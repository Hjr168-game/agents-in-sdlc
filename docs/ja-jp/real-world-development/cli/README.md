---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI のモード、カスタマイズ、MCP ツール、プルリクエスト自動化を学びながら、Tailspin Toys に 2 つの変更を加え、検証して公開します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** を使うと、ターミナルで GitHub Copilot をエージェント型のコーディングアシスタントとして利用できます。コードベースを調査し、コードを生成し、コマンドを実行し、外部ツールに接続します。すべてコマンドラインから行えるため、グラフィカルエディターに切り替えずに作業に集中できます。

このワークショップでは、Tailspin Toys の一連の作業を進めます。

- GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして、基本を確認します。
- 星評価の変更を加え、ブラウザーでレビューし、最初のプルリクエスト (PR) を手動でマージします。
- フィルタリングの issue を起点に、Plan モードで方針を決め、Autopilot モードで実装してから、Interactive モードでレビューします。
- リポジトリのカスタムインストラクションを更新し、フィルタリングの作業に適用します。
- 既存の `quality-checks` スキルをカスタマイズして、プロジェクトのチェックを実行します。
- Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーでフィルタリングの動作を確認します。
- 品質保証 (QA) 用のカスタムエージェントを作成して使い、要件、テスト範囲、検証結果をレビューします。
- フィルタリングの変更全体をレビューし、Agent Merge を使ってフィルタリングの PR をマージします。
- コンテキスト、モデル、共有、省略可能なクラウドへの委任に使うスラッシュコマンドを確認します。

このワークショップでは、星評価とフィルタリングの 2 つの PR を作成します。フィルタリングの PR には、カスタムインストラクション、スキルの更新、QA プロファイル、テストが含まれます。フィルタリングと品質確認の作業では、会話とブランチを 1 つにまとめ、各ツールを試しながら作業を積み重ねます。

## レッスン

| レッスン | トピック | 説明 |
| ------ | ----- | ----------- |
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成します |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証し、基本を確認します |
| [2. 星評価を追加する: 手早く成果を出す][ex2] | 最初の変更 | 既存の評価と `null` の場合の代替表示を確認し、最初の PR をマージします |
| [3. エージェントモード: Plan と Autopilot][ex3] | エージェントモード | issue の機能を Plan モードで計画し、Autopilot で実装して、Interactive モードでレビューします |
| [4. カスタムインストラクションで Copilot を導く][ex4] | コンテキスト | インストラクションを確認・更新し、フィルタリングに適用します |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 繰り返し可能なチェック | 既存のスキルを確認し、レポート形式を変更して実行します |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーによる確認 | CLI で MCP を構成し、フィルタリングの動作を確認します |
| [7. QA エージェントを作成して使う][ex7] | 要件とテスト範囲 | 専門プロファイルを作成して選択し、最終的な検証結果をまとめます |
| [8. 機能の PR を作成してマージする][ex8] | レビューとマージ | 変更全体をレビューし、PR を作成して Agent Merge を使います |
| [9. GitHub Copilot CLI のスラッシュコマンド][ex9] | CLI の機能 | コンテキスト、モデル、共有、クラウドエージェントへの省略可能な委任を確認します |
| [10. まとめと次のステップ][ex10] | まとめ | ワークフロー、再利用可能なカスタマイズ、今後のリソースを確認します |
| [省略可能: Foundry を組み込む][foundry] | ホスト型エージェント | モデルを準備し、カタログに基づくエージェントをデプロイしてサイトに接続します |

## 前提条件

このワークショップに参加する前に、次を準備してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかの有効なプランがある GitHub アカウント
- [ ] リポジトリと Codespace を作成する権限
- [ ] ターミナル / コマンド ライン操作の基本的な知識

> [!TIP]
> 有料プランをお持ちでない場合、認定を受けた学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれています。


> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合は、管理者が Copilot CLI を有効にしていることを確認してください。

## はじめる

**[前提条件から始める →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
