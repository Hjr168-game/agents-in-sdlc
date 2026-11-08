---
title: "演習 1 - カスタムインストラクション (クラウドエージェント)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot クラウドエージェント →][next-lesson]

生成 AI を使う際は、コンテキストが重要です。特定の方法でタスクを実行したい場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[インストラクションファイル][instruction-files]を使うと、Copilot に何を実行するかだけでなく、どのように実行するかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタムインストラクションとパスで対象を指定したインストラクションファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体の新しい標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここでは更新前と更新後のプロンプトを比較しません。Copilot クラウドエージェントは GitHub の issue に対して非同期で作業するため、リアルタイムで並べて効果を示すのは難しいからです。インストラクションファイルの影響は、このハーネスの後半でクラウドエージェントが作成したプルリクエストをレビューする際に確認します。

## インストラクションファイル

### シナリオ

Tailspin Toys には、開発に関するガイドラインと要件があります。

- データレイヤーには、必ずユニットテストを用意します。
- UI はダークモードにし、現代的な見た目にします。
- コードのドキュメントには、TSDoc のドキュメントコメントを使います。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

インストラクションファイルを使い、これらの慣行に沿ってタスクを行うための情報を Copilot に与えます。

### カスタムインストラクション

カスタムインストラクションを使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードを生成できるようになります。希望するコーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体に適用するファイルと、特定のファイルの種類やタスクに適用するファイルを作成できます。

インストラクションファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関するすべてのリクエストで Copilot に送信される単一のファイルです。多くのチャットや CLI のリクエストに関係する、プロジェクト全体の情報を記載します。たとえば、技術スタック、開発対象の概要、ベストプラクティス、全般的なガイダンスなどです。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に使うインストラクションファイルです。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニットテストの作成などを対象にできます。

> [!NOTE]
> IDE で作業する場合、インストラクションファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot クラウドエージェントは、リポジトリ単位のインストラクションと、`applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方をコード生成時に使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準形式のインストラクションファイルにも対応しています。詳細は[カスタムインストラクションのサポート][custom-instructions-support]を参照してください。

### インストラクションファイルを管理するためのベストプラクティス

インストラクションファイルの作成方法をすべて説明することは、このワークショップの範囲外です。サンプルプロジェクトに含まれるファイルは、一般的な方法を示しています。

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

プロジェクトには、タスク別の `*.instructions.md` ファイルと、中核となる `copilot-instructions.md` が用意されています。エディターまたは GitHub の Web UI で確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** には **GitHub Actions Workflows** のガイダンスも含まれており、Copilot を使うすべての場面に適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどのインストラクションがあります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象のファイルを指定する glob であることを確認します。ここでは、たとえば `**/*.test.ts` に一致するすべての TypeScript テストファイルが対象です。
5. このプロジェクトのユニットテスト作成に関する指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などのほかのインストラクションファイルや、プロジェクト内の既存ファイルへのリンクがあります。これにより、複雑なインストラクションを再利用しやすい小さなファイルに分け、コード生成時の例を Copilot に示せます。ここにあるパスは、リポジトリのルートではなくインストラクションファイルを基準とします。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトの標準が記載されていますが、コード内のドキュメントをまだ求めていません。次の手順で TSDoc のドキュメントコメントとファイルヘッダーのルールを追加します。

## リポジトリの標準を追加する

ここで行う編集は 1 つだけです。コード内のドキュメントには TSDoc のコメントとファイルレベルのコメントヘッダーを使う、というプロジェクト全体のルールを追加します。後続の演習で issue を割り当てると、クラウドエージェントがこのルールを使います。

編集の前に、作業用のブランチを作成して切り替えます。クラウドエージェントは issue が対象とするブランチのインストラクションを読み取りますが、ここでは変更をきちんとコミットします。

1. Codespace のターミナルで、次のコマンドを実行します。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を与えるファイルです。コード提案を改善するために、コーディング標準を追加します。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。既存のコーディング標準に、コード内のドキュメントに関するルールがまだないことを確認します。
3. 既存の標準の直後に、ファイルヘッダーと TSDoc コメントを追加するための次の行を挿入します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ全体に適用する指示は `.github/copilot-instructions.md` に、言語やファイルの種類、タスクに特化した指示は `*.instructions.md` ファイルに記載できます。今回追加したドキュメントコメントのルールなど、プロジェクト全体の標準にはリポジトリ単位のファイルが適しています。

## インストラクションの更新をコミット、プッシュ、マージする

クラウドエージェントは、issue の対象ブランチにあるインストラクションファイルを読み取ります。次の演習で Copilot に issue を割り当てると、`main` からブランチが作られます。クラウドエージェントが更新したインストラクションを使うには、変更を `main` にマージする必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成してマージします。次の演習で issue に割り当てたとき、クラウドエージェントはこのインストラクションを読み取ります。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチの作成を省略して、変更を `main` にコミットできます。ここでは、実際のプロジェクトで行う方法に沿ってブランチを使っています。

## まとめと次のステップ

このプロジェクトのインストラクションファイルから Copilot がコンテキストを得る方法を確認し、リポジトリ全体に適用する新しい標準を `.github/copilot-instructions.md` に追加しました。クラウドエージェントが作成する以降のプルリクエストで、この標準が反映されます。

次は、Copilot クラウドエージェントに[最初の issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタムインストラクションファイル][instruction-files]
- [カスタムインストラクション作成のベストプラクティス][instructions-best-practices]
- [Copilot のカスタムインストラクションを改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — インストラクションファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
