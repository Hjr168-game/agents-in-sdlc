---
title: "演習 5: GitHub Copilot の作業を反復的に改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザーエクスペリエンスの向上や機能の追加を目的に、GitHub Copilot に複数の作業を依頼しました。コードにドキュメントを追加し、デザインチームが試せる関連ゲーム機能を実装し、ハイコントラストとライトモードの切り替えなどのアクセシビリティ機能を追加しました。変更を確認し、必要に応じて Copilot に改善を依頼しましょう。

### シナリオ

生成 AI を利用しても、ソフトウェア設計や DevOps の基本は変わりません。生成されたコードは必ずレビューし、通常の DevOps プロセスに沿って進めます。チーム全体のレビューを有効にする前に、Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能を確認しましょう。

## GitHub Copilot クラウドエージェントのセキュリティ

Copilot クラウドエージェントは非同期かつ自律的に作業するため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みできるのはコード用ブランチだけです。
- クラウドエージェントは GitHub Actions 内に、独立した一時的な環境を作成して実行されます。
- GitHub Actions のワークフローを実行するには、人間による承認が必要です。
- MCP サーバーを含む外部リソースへの[アクセスは既定で制限][agent-firewall]されています。

## 生成されたドキュメントをレビューする

まず、Copilot クラウドエージェントが作成した、コードにドキュメントを追加する最初のプルリクエスト (PR) を、GitHub.com の標準的な PR 画面で確認します。

> [!NOTE]
> PR に、GitHub Copilot がファイアウォールでブロックされたという警告が表示されることがあります。Copilot は MCP サーバーへのアクセスも含め、既定では外部リソースへのアクセスを制限されているため、これは**想定された動作**です。必要であれば、Copilot クラウドエージェントのファイアウォールを[カスタマイズまたは無効化][agent-firewall]できます。

1. github.com でリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルの PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。作業が完了するまで待ちます。数分かかることがあるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選択し、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc コメントやその他のドキュメントを確認します。具体的な変更内容は異なる場合があります。

    変更を確認しながら、TSDoc コメント、TypeScript の慣習、コメントヘッダーに注目してください。これらは演習 2 の最初に確認したカスタムインストラクションファイルに基づいています。

6. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローが承認待ちの場合は **Approve and run workflows** を選択します。

    ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローが追加されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

Copilot との PR 作業は一方通行ではありません。チームメンバーと同様に、PR やコードへのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するための新しいセッションを開始します。結果は予測できないため、必ず特定の文面を指定することはできません。依頼できる変更の例を示します。

- 各コードファイルの先頭に、ファイルの役割を説明するコメントヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc コメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 上記の例や、コードベースに追加したいドキュメントのアイデアを使って、**@copilot** をメンションしたコメントを投稿します。
2. **View Session** を選択して Copilot の作業を確認します。Copilot は更新用の新しいセッションを開始します。
3. **Back to pull request** を選択して PR に戻れます。

    ![プルリクエストに戻る操作](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、PR に新しいコミットが表示されます。
5. **Files changed** タブを選択して変更をレビューします。

納得できるまで反復できます。問題がなければ、ドラフト PR をレビュー可能な状態にして、`main` ブランチにマージします。

![PR をレビュー可能な状態にする操作](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲームの詳細ページに関連ゲームを表示する issue に対して、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択して生成されたコードをレビューします。
5. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す表示を確認します。
7. 承認待ちの場合は **Approve and run workflows** を選択します。

    ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. PR のチェック欄にワークフローが追加されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
9. **省略可能:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動テストできます。Codespace に移動し、ターミナルを開いて、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。クラウドエージェントにコメントを追加すると、ほかの PR と同様に反復的な更新を依頼できます。たとえば、表示する関連ゲームの数を変更したり、カスタムインストラクションの更新**前**に割り当てたため不足していたコメントヘッダーや TSDoc コメントを追加したりできます。**Conversation** タブにコメントを追加すると、Copilot はそれを受け取って新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタムエージェントが実装した機能を確認します。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 のミッションコントロールで追加を依頼したライトモードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。作業が完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択してコードをレビューします。
5. 特に次の点を確認します。
   - モード切り替え用の UI コンポーネント
   - ユーザー設定を保存するローカルストレージの使用方法
   - ハイコントラストモードとライトモードの CSS またはスタイルの変更
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)
   - モードの切り替えを管理する JavaScript/TypeScript コード

6. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

    ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローが追加されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
10. **省略可能:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace のターミナルを開き、Copilot が作成したブランチ名に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次にアプリを起動し、ブラウザーでハイコントラストモードとライトモードを切り替え、ページを再読み込みしても設定が維持されることを確認します。

アクセシビリティ用カスタムエージェントは、アクセシビリティのベストプラクティスに沿って機能を実装するよう Copilot を導きます。懸念や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 省略可能な演習 — 委任を続ける

クラウドエージェントには、実際のバックログ作業を任せ、その間にほかの作業に取り組む方法が適しています。この習慣を身につけるために、リポジトリでいくつか issue を作成して Copilot に割り当ててみましょう。

- ゲームの詳細ページに支援者向けの関心フォームを作成します。
- ゲーム一覧ページにページネーションを実装します。
- データアクセスヘルパーに入力検証とエラー処理を追加します。

## まとめ

クラウドエージェントのハーネスを完了しました。各レッスンを通じて、次のことを行いました。

- **このリポジトリに含まれるカスタムインストラクションファイルを確認し**、クラウドエージェントの出力への影響を確認しました。
- **issue を Copilot クラウドエージェントに割り当て**、環境のセットアップ、計画、非同期の作業を確認しました。
- **アクセシビリティ用カスタムエージェントを作成して使い**、ハイコントラストモードとライトモードの切り替えを追加しました。
- **Copilot エージェントページをミッションコントロールとして使い**、作業中のアクセシビリティセッションを監視して方向付けました。
- **クラウドエージェントの PR をレビューして反復的に改善し**、`@copilot` に変更を依頼してワークフローを承認しました。

## レビューと次のステップ

クラウドエージェントのハーネスを完了しました。他のハーネスでは、学んだ内容を別の環境で試せます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat のエージェントモードと MCP 連携を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、省略可能な `/delegate` によるクラウドエージェントとの連携を使い、ターミナルからレビュー済みの変更を 2 つ公開します。

自分のリポジトリでは、次のことも試してみましょう。

- リファクタリングやテスト範囲拡大の issue をクラウドエージェントに割り当てます。
- 別の分野向けにカスタムエージェントを作成します。
- 異なる技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot クラウドエージェントのセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot クラウドエージェントのファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
