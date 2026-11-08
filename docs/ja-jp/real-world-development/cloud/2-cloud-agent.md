---
title: "演習 2 - GitHub Copilot クラウドエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタムインストラクション][previous-lesson] |
|:--|

多くの組織では、解決されていないセキュリティ課題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能要望など、技術的負債を抱えています。GitHub Copilot のクラウドエージェントは、コードの更新や機能の追加などを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を任せて開発を加速し、より重要な作業に集中できます。

Copilot クラウドエージェントを使って、次のことを学びます。

- コード生成環境のカスタマイズ
- 安全に操作を実行する方法
- 範囲を明確に定めた issue の重要性
- Copilot への issue の割り当て

## シナリオ

Tailspin Toys は技術的負債に取り組みたいと考えています。最初のサイトを作成した委託先はドキュメントを十分に作成していませんでした。まず、アプリケーションのエクスポートされたすべての関数に TSDoc のドキュメントコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。ゲームの詳細ページに同じカテゴリの関連ゲームを表示し、閲覧を続けられるようにします。まだ完成度の高い実装は必要なく、UX の受け入れテストに使える状態を求めています。この作業は妨げになっていますが、より優先度の高い issue もあります。

どちらも、優先順位が下がりやすいタスクです。Copilot クラウドエージェントに割り当てれば、非同期で作業を進めてもらい、その間にほかの作業に取り組み、完了後に結果をレビューできます。

## GitHub Copilot クラウドエージェントについて

[GitHub Copilot クラウドエージェント](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)は、人間の開発者と同様にバックグラウンドで作業できます。たとえば、[GitHub issue を Copilot に割り当て][assign-issue]ます。割り当てると、Copilot は進行状況を追跡するドラフトのプルリクエストを作成し、環境をセットアップして、タスクに取りかかります。実行中でも完了後でもセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確に定めることの重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単そうな操作も、詳しく調べると複雑なことがあります。[Copilot クラウドエージェントに依頼するタスクの範囲を明確に定めてください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階的に進め、学び、試し、必要に応じて調整します。生成 AI が加わっても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタムインストラクション

前の演習では、カスタムインストラクションが Copilot を導く方法を学びました。まだ `.github/copilot-instructions.md` を見ていない場合は、クラウドエージェントに作業を割り当てる前に、リポジトリに含まれるインストラクションファイルを読み取り専用で確認してください。後で影響を確認できます。

GitHub の Web UI または起動中の Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` の **Code standards** セクションを確認します。特に TypeScript の慣習と TSDoc のドキュメントコメントに関する規則を確認します。
- `.github/instructions/unit-tests.instructions.md` を開きます。`applyTo` frontmatter が、`**/*.test.ts` に一致するファイルにインストラクションを適用することを確認します。

次のセクションで `Code lacks documentation` issue をクラウドエージェントに割り当てたら、作成されたプルリクエストで TSDoc コメント、TypeScript の慣習、コメントヘッダーを確認してください。これらの指示は後のレッスンでプルリクエストをレビューする際にも取り上げます。

## Copilot クラウドエージェントの開発環境を設定する

誰が作業する場合でも、コードを作成するには通常、環境を整え、状態を正常に保つセットアップスクリプトを実行する必要があります。Copilot にタスクを割り当てる場合も同じです。Copilot もソフトウェアエンジニアのように作業します。

クラウドエージェントは、作業環境として [GitHub Actions][github-actions]を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップワークフロー][setup-workflow]を作成すると、作業前に実行される環境をカスタマイズできます。これにより、必要な開発ツールと依存関係を使えるようになります。このラボのために、すでに次のものを利用できるよう設定されています。Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、単一 Astro アプリ用にマイグレーションとシードを済ませた SQLite データベースです。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 手順に必要な最小限の権限を設定します。Copilot の操作には専用のトークンが使われます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順でリポジトリをクローンする場合は `contents: read` 権限が必要です。クローンしない場合は、手順の完了後に Copilot が自動で行います。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL です)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテスト用のデータを用意するため、ローカル SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub Actions ワークフローと同じように見えますが、重要な点が 2 つあります。

- `copilot-setup-steps` という単一のジョブがあり、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot の実行を待たずにテストできるため便利です。

## ドキュメントを追加する

多くのプロジェクトでは、ドキュメントが古いか、まったくないことがあります。この種の技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメント作成が得意なので、クラウドエージェントに依頼するのに適したタスクです。バックグラウンドで必要なドキュメントを作成してもらい、後の演習で結果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次のテキストを入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てのダイアログを開きます。

    ![Copilot を issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

    ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような内容です。一覧に PR が表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が todo リストを作成したことを確認します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで数分かかることがあります。Copilot は先ほどのワークフローを実行して環境を作り、プロジェクトを分析して、問題に取り組む方法を決めています。

13. todo リストと、実行予定の作業を確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

    ![Copilot のセッションビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示が出るまで、ウィンドウを更新する必要がある場合があります。

16. ライブセッションをスクロールして、Copilot が問題を解決する様子を確認します。コードを調査して状態を把握し、計画を考えて判断し、コードを作成する様子が表示されます。

完了まで数分かかることがあります。クラウドエージェントの主な利点は、非同期で作業を任せて、ほかの作業に集中できることです。この機能を使って、もう 1 つのタスクをクラウドエージェントに割り当て、その間にアプリへ機能を追加します。

## ゲームの詳細ページに関連ゲームを追加する

GitHub Copilot クラウドエージェントの利点の 1 つは、作業を分担できることです。デザインチーム向けの関連ゲームの追加に時間がかからないとしても、その時間をほかの作業に充てられます。クラウドエージェントに割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択します。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てのダイアログを開きます。

    ![Copilot を issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot が作業中であることを示す目の絵文字 👀 が issue の最初のコメントに表示されます。

![Copilot が issue に取り組んでいることを目の絵文字で示す画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。クラウドエージェントはソフトウェアエンジニアと同じように作業するため、完了まで常に監視する必要はありません。完了後にレビューできます。次はカスタムエージェントの作成と使用を確認しましょう。

## まとめと次のステップ

[GitHub Copilot クラウドエージェント][copilot-agents]を使うと、issue を割り当てて非同期に作業を進められます。技術的負債への対応、新機能の作成、フレームワーク間のコード移行などに利用できます。

このレッスンでは、次のことを学びました。

- コード生成環境をカスタマイズする方法
- 操作を安全に行う方法
- 範囲を明確に定めた issue の重要性
- Copilot に issue を割り当てる方法

クラウドエージェントがバックグラウンドで作業している間に、カスタムエージェントを作成して使ってみましょう。[Copilot クラウドエージェントは MCP サーバー][cloud-agent-mcp]とカスタムインストラクションも利用できます。これらは前のモジュールで確認しました。

## リソース

- [Copilot クラウドエージェントについて][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot クラウドエージェントのセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
