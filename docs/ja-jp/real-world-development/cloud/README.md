---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot クラウドエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[GitHub Copilot クラウドエージェント](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)を使うと、GitHub Copilot がクラウド上で非同期に作業できます。GitHub で作業を割り当てると、クラウドエージェントはバックグラウンドでリポジトリを調査し、変更を加えてプルリクエストを作成します。その間、ほかの作業を続けられます。

このハーネスでは、まずクラウドエージェントが従うカスタムインストラクションを追加し、GitHub issue を割り当てて実装を任せます。カスタムエージェントを確認して利用し、エージェントダッシュボードからセッションを監視して方向付けます。最後に、作成されたプルリクエストをレビューし、結果を反復的に改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成します |
| [1. カスタムインストラクション][ex1] | コンテキスト | クラウドエージェントが従うカスタムインストラクションを追加します |
| [2. クラウドエージェント][ex2] | 非同期エージェント | issue を Copilot クラウドエージェントに割り当てます |
| [3. カスタムエージェント][ex3] | 特化エージェント | カスタムエージェントを確認して利用します |
| [4. エージェントの管理][ex4] | 監視 | エージェントのセッションを監視して方向付けます |
| [5. 反復的な改善][ex5] | レビュー | PR をレビューし、Copilot の作業を反復的に改善して、次の手順を選びます |

## 前提条件

このワークショップに参加する前に、次の準備をしてください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランの GitHub アカウント
- [ ] アカウントまたは組織で Copilot クラウドエージェントが有効になっていること

> [!TIP]
> 有料プランをお持ちでない場合、認定を受けた学生は[GitHub Education][callout-student-plan-education]を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれているため、どのハーネスも完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> クラウドエージェントは、Copilot Free を除く **Copilot Student、Pro、Pro+、Business、Enterprise** のすべてのプランで利用できます。Copilot Business または Enterprise では、管理者が組織に対して有効にする必要があります。

> [!NOTE]
> このハーネスでは MCP を扱いません。Copilot で MCP サーバーを使う方法については、[CLI ハーネス](../cli/)または [VS Code ハーネス](../vscode/)を参照してください。

## はじめに

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
