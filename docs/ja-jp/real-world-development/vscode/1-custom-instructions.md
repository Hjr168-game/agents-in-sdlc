---
title: "演習 1 - カスタムインストラクション (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使う際は、コンテキストが重要です。タスクを特定の方法で行う必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot に特定の情報を伝える方法はいくつかあります。その中でも重要なのが[インストラクションファイル][instruction-files]です。コード生成の指針を与えられます。

この演習では、次のことを行います。

- リポジトリのカスタムインストラクションとパスで対象を指定したインストラクションファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- 現在のインストラクションに従って、フィルタリングに使う最初のデータ部分 (発行元のヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体の新しい標準を追加します。
- 同じプロンプトを再実行し、生成コードに新しい標準が反映される様子を確認します。
- インストラクションとフィルタリングの最初の実装を `main` にコミットし、次の演習で更新後のガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードは、設定した標準と異なる場合があります。Copilot の出力は確定的ではありません。この演習では、更新後に動作がどのように変わるかを確認します。出力を一字一句同じにすることが目的ではありません。

## インストラクションファイル

### シナリオ

Tailspin Toys には、開発に関するガイドラインと要件があります。

- データレイヤーには、必ずユニットテストを用意します。
- UI はダークモードにし、現代的な見た目にします。
- コードのドキュメントには、TSDoc のドキュメントコメントを使います。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

インストラクションファイルを使い、これらの慣行に沿った作業を行うための情報を Copilot に与えます。

### カスタムインストラクション

カスタムインストラクションを使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードを生成できるようになります。希望するコーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体に適用するファイルと、特定のファイルの種類やタスクに適用するファイルを作成できます。

インストラクションファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関するすべてのリクエストで Copilot に送信される単一のファイルです。技術スタック、開発対象の概要、ベストプラクティスなど、プロジェクト全体に関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に使うインストラクションファイルです。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニットテストの作成などを対象にできます。

> [!NOTE]
> IDE で作業する場合、インストラクションファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot クラウドエージェントは、リポジトリ単位のインストラクションと、`applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方をコード生成時に使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準形式のインストラクションファイルにも対応しています。詳細は[カスタムインストラクションのサポート][custom-instructions-support]を参照してください。

### インストラクションファイルを管理するためのベストプラクティス

インストラクションファイルの作成方法をすべて説明することは、この演習の範囲外です。サンプルプロジェクトに含まれるファイルは、一般的な方法を示しています。

- `copilot-instructions.md` には、開発対象の説明、プロジェクト構成、全体のコーディング標準など、プロジェクト全体に関係するガイダンスを記載します。
- `*.instructions.md` ファイルには、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイルの種類や特定のタスクに関する指示を記載します。
- 自然な言葉を使い、ガイダンスを明確にします。コードのよい例と避けるべき例を含めます。

インストラクションファイルの作成方法は 1 つではありません。AI の使い方と同様に、試しながらプロジェクトに合った方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、充実したインストラクションファイルを用意すると役立ちます。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスクに対応するファイルがあります。
>
> Copilot にインストラクションファイルを生成させることもできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` など、各環境で方法が異なります。必要な場合は、利用している環境のレッスンで説明します。
>
> テンプレートや出発点を探している場合は、インストラクションファイル、カスタムエージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot]を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタムインストラクションファイルを確認する

このスタータープロジェクトには、`copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルが含まれています。エディターまたは GitHub の Web UI で確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** には **GitHub Actions Workflows** のガイダンスも含まれており、Copilot を使うすべての場面に適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどのインストラクションがあります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象のファイルを指定する glob であることを確認します。ここでは、たとえば `**/*.test.ts` に一致するすべての TypeScript テストファイルが対象です。
5. ユニットテストの作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などのほかのインストラクションファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きなインストラクションを再利用しやすい小さなファイルに分け、Copilot に例を示せます。ここにあるパスは、リポジトリのルートではなくインストラクションファイルを基準とします。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトの標準が記載されていますが、コード内のドキュメントを求める規則はまだありません。次の手順で TSDoc のコメントとファイルヘッダーのルールを追加します。

## 変更用のブランチを作成する

まず、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 次のコマンドでブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## インストラクションの更新前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な場合は各手順で説明します。

まず現在のインストラクションを使ってプロンプトを送信し、カスタムインストラクションの影響を確認します。後でインストラクションを更新して、同じプロンプトをもう一度送信します。

1. Copilot が必要なコンテキストだけを使うよう、前の演習で開いたエディタータブを閉じます。
2. ヘルパーの配置先として `src/lib/publishers.ts` を開きます。
3. Chat ビューのエージェント選択メニューで **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択メニューのスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べ、ヘルパーファイルやテストなどのコードを更新します。
6. 提案されたヘルパーは型付き関数です。最初の引数に `db` クライアントを受け取り、発行元の型付き配列を返します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤーの慣習に基づいています。
7. 提案されたコードには TSDoc コメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot の出力は確率的であるため、指示しなくてもドキュメントコメントが追加されることがあります。その場合も問題ありません。ここではインストラクションを更新した後の一貫性の向上に注目します。

## リポジトリの標準を追加する

`.github/copilot-instructions.md` にプロジェクト全体の情報を記載し、コード提案を改善するためのコーディング標準を追加します。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。既存の標準には、生成されたヘルパーにドキュメントコメントがなかった理由となるコード内ドキュメントのルールがありません。
3. 既存の標準の直後に、ファイルヘッダーと TSDoc コメントを追加する次の行を挿入します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> リポジトリ全体に適用する指示は `.github/copilot-instructions.md` に、言語やファイルの種類、タスクに特化した指示は `*.instructions.md` に記載できます。今回追加したドキュメントコメントのルールなど、プロジェクト全体の標準にはリポジトリ単位のファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話をクリアします。
2. `src/lib/publishers.ts` を選択し、Copilot に対象ファイルを認識させます。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックが追加されたことを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 関数に次のような TSDoc コメントが追加されたことを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準に従うよう Copilot を導き、次の演習で使う実際のコードに適用しました。

## インストラクションをコミットしてブランチをプッシュする

インストラクションファイルもほかの資産と同じように、ソース管理で管理します。変更をコミットしてブランチをリポジトリにプッシュします。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押し、新しいターミナルを開きます。
2. 次のコマンドを実行し、インストラクションとヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. インストラクションとヘルパーの基盤に関する変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の変更 (テストなど) が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

プッシュしたブランチからプルリクエストを作成し、バックログにあるドキュメント標準の issue に関連付けます。Copilot に issue を探させ、issue をクローズする PR を作成してマージを依頼します。

> [!NOTE]
> Copilot に GitHub 上の操作を依頼するのはこれが初めてです。プロジェクトテンプレートでは GitHub のツールがあらかじめ接続されています。Copilot が初めてツールを使う際に、VS Code で GitHub へのサインインを求められる場合があります。画面の指示に従って許可してください。後の演習で、この接続の仕組み (Model Context Protocol) を学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. インストラクション更新と発行元ヘルパーの基盤を説明する PR を現在のブランチから作成するよう、Copilot に依頼します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を探して PR を作成します。
5. GitHub の操作を許可するよう求められたら、コマンドを確認して、適切であれば **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージしてローカルブランチを `main` に戻すよう依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub の操作やシェルコマンドの実行を許可するよう求められたら、内容を確認し、適切であれば **Allow** を選択します。

GitHub Copilot の助けを借りて PR を作成し、マージできました。

## まとめと次のステップ

Copilot がインストラクションファイルからコンテキストを得る方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存のインストラクションを使ったコード生成プロンプトを送信し、結果を確認しました。
- `.github/copilot-instructions.md` に新しい標準を追加しました。
- 同じプロンプトを再実行し、提案コードに新しい標準が反映される様子を確認しました。
- 次の演習で使えるよう、インストラクションとヘルパーの基盤を `main` にコミットしました。

次は、Copilot がコードベース全体に新機能を追加する際に、[エージェントモードでインストラクションを適用します][next-lesson]。

## リソース

- [GitHub Copilot のカスタムインストラクションファイル][instruction-files]
- [カスタムインストラクション作成のベストプラクティス][instructions-best-practices]
- [Copilot のカスタムインストラクションを改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタムインストラクション][personal-instructions]
- [Awesome Copilot — インストラクションファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
