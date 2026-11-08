---
title: "第 2 课 - 添加星级评分：快速改进"
description: "在 GitHub Copilot app 中启动第一个智能体会话，对游戏卡片进行一项小改动，并通过第一个拉取请求合并更改。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

浏览工作区并尝试快速聊天后，现在可以启动**智能体会话**，对项目进行第一次更改。这次改动很小：游戏数据中已有星级评分，但主页上的游戏卡片尚未显示。你将让智能体显示已有评分、审查更改，并将其作为第一个拉取请求合并。

本课将学习如何：

- 启动智能体会话，并了解会话的结构。
- 让智能体对项目进行一项范围明确的小改动。
- 在工作区差异视图中审查更改。
- 在本地运行应用，并在浏览器中确认更改。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都可以有星级评分，游戏详情页已经显示该评分。不过，主页上的游戏卡片仅显示标题、类别、发行商和说明。作为热身，你将让智能体在每张卡片上显示已有评分。这项小型且独立的更改非常适合作为第一个会话任务。

## 会话剖析

**会话**是与智能体的对话，在独立工作区中运行。每个会话都有**专用的 git 工作树和分支**，因此可以同时运行多个会话，例如一个添加功能，另一个修复 bug，而不会造成更改冲突。会话按存储库分组显示在侧边栏中，选择任一会话即可切换。

会话中包含三类内容：与智能体的**对话**、智能体探索和编辑文件时的**工具活动**，以及带有差异的**已更改文件**列表。

## 启动会话并请求更改

现在启动新会话，开始探索项目并实现功能。在[应用设置期间][prior-lesson]，你已从 GitHub 存储库添加项目。现在为该存储库创建新会话并请求更改。

1. 返回（或打开）GitHub Copilot app。
2. 选择侧边栏顶部的 **+**，启动会话。
3. 选择 `tailspin-toys` 存储库。
4. 在提示框下方选择 **new working tree** 和 **Interactive** 模式。使用以下提示词请求更改：

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. 按 <kbd>Enter</kbd> 将提示词发送给 Copilot。

Copilot app 会先创建新的工作树，即项目的隔离副本。随后，它会探索项目，找到实现新功能所需更新的文件并生成代码。现在已使用 Copilot app 添加了一项新功能。

## 审查差异

所有 AI 生成的更改在合并前都应接受审查，即使更改很小也是如此。现在直接在 Copilot app 中检查更改。

1. 在应用右上角选择 **Toggle review panel**。在打开的面板中选择 **Changes**，查看已更改文件及其差异。

    ![GitHub Copilot app 顶部工具栏，箭头指向 Create PR 右侧的 Toggle review panel 按钮](../../../_images/app-2-review-panel.png)

2. 应该会看到用于显示游戏详情的核心文件 `GameCard.astro` 中新增了代码。代码应类似以下内容：如果评分存在则显示评分，如果 `starRating` 为 `null` 则显示 "No rating yet"：

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 具有概率性而非确定性，因此生成的代码可能与以上示例不同，但应大致相似。

## 检查更改

创建浏览器前，先审查智能体的自动检查结果。确认测试覆盖数值类型的 `starRating` 和 `null` 回退状态。缺少先决条件或跳过的检查不算通过；批准任何安装请求前请先审查。

不要只阅读代码并假设它可以正常工作。让 Copilot 打开网站，在浏览器画布中检查更新后的 UI。

> [!TIP]
> 画布是 Copilot app 内的交互式小组件。稍后会探索自定义画布，甚至创建自己的画布；现在先使用内置的浏览器画布。

1. 使用以下提示词，让 Copilot 启动应用并在浏览器画布中打开页面：

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. 稍等片刻，应用将启动，Copilot app 内会打开浏览器窗口。
3. 确认已评分的游戏卡片显示满分为五分的评分值。
4. 完成后，使用以下提示词让 Copilot 停止为此会话启动的开发服务器，并关闭浏览器画布：

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## 创建并合并第一个拉取请求

你已创建该功能。现在创建拉取请求 (PR)，将新代码合并到现有代码库。

1. 选择右上角的 **Create PR**。
2. 如果系统提示，请选择 **Sign in with your browser**，并按照提示完成身份验证。
3. 如果 Copilot 说明存储库策略要求先关联议题才能创建 PR，请确认让它遵循此流程。
4. Copilot 会开始创建 PR。
5. 选择聊天上方的 **PR** 气泡，在审查面板中打开 PR 并查看内容。可根据需要在此进行审查。
6. 准备就绪后，选择 **Ready to merge**。
7. 在新对话框中选择 **Merge pull request**，合并 PR。

## 总结和后续步骤

恭喜！你已使用 GitHub Copilot app 交付第一个更改。本课完成了以下工作：

- 启动智能体会话并了解会话的结构。
- 引导智能体对游戏卡片进行范围明确的小改动。
- 在工作区差异视图中审查更改。
- 在本地运行应用，并在浏览器中确认星级评分。
- 打开 PR 1、审查检查结果并明确执行合并。

下一课将[从筛选议题开始，使用 Plan 和 Autopilot 模式构建更大的功能][next-lesson]。

## 资源

- [在 GitHub Copilot app 中使用智能体会话][agent-sessions]
- [关于 GitHub Copilot app][about-copilot-app]
- [使用 GitHub Copilot app 管理议题和拉取请求][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#安装并配置-github-copilot-app
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
