---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几课中，你已让 Copilot 云智能体执行三项任务，以改善用户体验并添加功能。云智能体可以异步、自主地工作，但仍需要监控这些任务。

可以通过多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心中，可以查看所有有未关闭拉取请求（PR）的智能体任务、了解已执行的操作，甚至引导正在进行的会话。

本课将学习如何：

- 使用智能体页面监控云智能体任务。
- 在会话进行期间引导智能体，实现其他功能。

## 场景

将任务分配给智能体以创建高对比度模式后，团队意识到也可以同时添加浅色模式。由于网站样式和切换功能正在更新，加入此功能也很合适。你将引导智能体，使其在添加高对比度模式的同时也实现浅色模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务的状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主区域和左侧区域中的任务列表。列表应包含分配给 Copilot 的任务，例如：
   - 更新代码文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，包括：
   - 从存储库检出代码。
   - 为 Copilot 创建环境。
   - 设置 MCP 服务器。
   - 执行完成所分配任务的各种操作。

> [!NOTE]
> 具体显示的步骤会因 Copilot 当前的工作状态和采用的方法而异。

4. 同时查看右侧显示的拉取请求（PR）面板。可以通过它查看 PR 和已更改文件，进一步监控任务。

## 引导云智能体

现在已经看到正在运行的任务，接下来要求 Copilot 在处理高对比度模式时，也添加浅色模式切换开关。

1. 选择与添加高对比度模式相关的会话。会话标题会因 Copilot 使用的名称和当前工作状态而异。

    ![任务控制中心中的无障碍访问会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到设置完成并开始执行任务。此时会出现类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示词：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中的云智能体任务截图，突出显示“Steer active session while Copilot is working”对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示词。
5. 注意 Copilot 会确认收到提示词，并将其纳入工作流程。

## 等待 Copilot 完成工作

与之前一样，Copilot 会继续处理更新后的任务！发送消息时，Copilot 会先完成正在执行的步骤，再将新请求加入工作流程。

此过程可能需要几分钟，可以利用这段时间回顾已学内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，它是监控和引导 GitHub Copilot 云智能体任务的中心位置。借助任务控制中心，可以跟踪所有正在进行和已完成的任务、审查已执行的工作，甚至在会话进行期间重定向任务，以调整范围或提供更多指导。

本课介绍了以下概念：

- 使用任务控制中心和智能体页面监控云智能体任务。
- 重定向正在进行的会话，要求添加其他功能。

Copilot 正在完成无障碍功能。接下来，审查 [Copilot 创建的拉取请求并迭代改进][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
