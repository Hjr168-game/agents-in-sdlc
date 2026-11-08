---
title: "レッスン 4 - カスタムインストラクションで Copilot を導く"
description: "リポジトリのインストラクションを確認し、ドキュメント標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使う際は、コンテキストが重要です。特定の方法でタスクを行う必要がある場合、そのガイダンスを Copilot が参照できるようにします。[インストラクションファイル][instruction-files]には、求めるコードだけでなく、その構成方法も記述できます。フィルタリング機能を実装できたので、Copilot が参照したインストラクションを確認し、ドキュメントの標準を追加してコードに適用します。

このレッスンでは、次のことを行います。

- リポジトリのインストラクションとパスで対象を指定したインストラクションファイルが、エージェントに適用される方法を確認します。
- コーディング標準に従うようインストラクションファイルを更新します。
- インストラクションファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発に関するガイドラインと要件があります。

- コメントには、コードの説明を繰り返すのではなく、意図や自明ではない判断を記述します。
- `db/` と `src/lib/` にあるエクスポートされた関数では、`db` 引数を注入できる場合はその点も含め、目的、パラメーター、戻り値を TSDoc/JSDoc で説明します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明し、関連コードが変更された際にコメントも最新の状態に保ちます。
- 既存の書式と lint のガイダンスを維持します。

インストラクションファイルを使い、これらの慣行に沿った作業を行うための情報を Copilot に与えます。

## インストラクションファイル

カスタムインストラクションを使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードを生成できるようになります。希望するコーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体に適用するファイルと、特定のファイルの種類やタスクに適用するファイルを作成できます。

インストラクションファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関するすべてのリクエストで Copilot に送信される単一のファイルです。多くのチャットや CLI のリクエストに関係する、プロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、タスクに関するガイダンスを記載します。

> [!NOTE]
> ほかの形式のインストラクションとそのサポート状況は、環境によって異なります。特定の形式を使用する前に、[カスタムインストラクションのサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタムインストラクションファイルを確認する

スタータープロジェクトには、すでに複数のインストラクションファイルが含まれています。変更を加える前に内容を確認し、変更後の影響と比較しましょう。

1. Codespace に戻ります。
2. Codespaces エディターで、ターミナルではなく `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングガイダンスを確認します。このインストラクションは、リポジトリ内で Copilot と行うすべてのやりとりに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データレイヤー、テストなどのインストラクションがあります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象のファイルを指定する glob であることを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかのインストラクションファイルや既存のプロジェクトファイルへのリンクを確認します。これにより、大きなインストラクションを再利用しやすい小さなファイルに分け、Copilot に例を示せます。

## チームのガイダンスに合わせてインストラクションを更新する

既存のファイルはよい出発点ですが、追加で対応する点があります。中核となる `copilot-instructions.md` を変更し、新しく生成する TypeScript に TSDoc コメントを追加させます。

1. `.github/copilot-instructions.md` で、**Code formatting guidance** というセクションを探します。35 行目付近にあります。
2. セクションの最後に、次の項目を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

変更は自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は、会話の開始時にリポジトリのインストラクションを読み込みます。更新を保存したら、フィルタリングの会話を終了して再開します。新しいガイダンスを読み込んだ状態で、機能に関するコンテキストを保てます。

1. フィルタリングの会話が開いている場合は、`/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリルートで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   会話の選択を求められた場合はフィルタリングの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。

3. Copilot に次のように依頼して、更新したガイダンスを適用します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力して、変更された TypeScript ファイルを確認します。新しく追加された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI がインストラクションファイルからコンテキストを読み取る方法を確認し、機能に新しい標準を適用しました。具体的には、次のことを行いました。

- リポジトリのインストラクションとパスで対象を指定したインストラクションファイルが、エージェントに適用される方法を確認しました。
- コーディング標準に従うようインストラクションファイルを更新しました。
- インストラクションファイルがコードに与える影響を確認しました。

次は、再利用できる `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、lint とテストを一貫して実施します。

## リソース

- [Copilot CLI にカスタムインストラクションを追加する][instruction-files]
- [カスタムインストラクションのサポート][custom-instructions-support]
- [カスタムインストラクションを作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — インストラクションファイルなどのコレクション][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
