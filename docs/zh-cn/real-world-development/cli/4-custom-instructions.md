---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "探索存储库指令、添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，应确保 Copilot 能访问相应指导。[指令文件][instruction-files]不仅说明希望编写什么代码，也说明代码应该如何组织。现在筛选功能已经完成，可以查看 Copilot 使用的指令、添加文档标准，并将其应用到代码中。

本课将学习如何：

- 查看智能体如何获取存储库指令和按路径限定范围的指令文件。
- 更新指令文件，确保遵循编码标准。
- 了解指令文件对代码的影响。

## 场景

Tailspin Toys 制定了一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值；如果有可注入的 `db` 参数，也应说明。
- 可复用 Astro 组件应记录 `Props` 接口约定，并在相关代码更改时同步更新注释。
- 应保留现有格式和 lint 指南。

通过指令文件，可以确保 Copilot 了解这些要求，并按照项目实践完成任务。

## 指令文件

自定义指令可向 Copilot 提供上下文和偏好设置，帮助它理解你的编码风格和要求。借助这些指令，可以引导 Copilot 提供更相关的建议和代码片段。你可以为整个存储库设置指令，也可以针对特定文件类型或任务设置指令。

指令文件有两种类型：

- `.github/copilot-instructions.md`：每次针对该存储库向 Copilot 提交请求时，都会发送此单一指令文件。它应包含大多数请求都适用的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导的文件。

> [!NOTE]
> 其他指令格式及其支持情况会因环境而异。依赖特定格式前，请参阅[自定义指令支持参考][custom-instructions-support]。

## 浏览本项目的自定义指令文件

入门项目已附带一组指令文件。修改文件并观察效果前，先查看其中现有内容。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，注意项目简介和编码指导。此文件适用于在该存储库中与 Copilot 的所有交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。你会看到针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了一个 glob，用于确定适用的文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看其中指向其他指令文件和项目现有文件的链接。这样可以将较大的指令集拆分成较小、可复用的文件，并为 Copilot 提供可参考的示例。

## 更新指令文件以符合团队要求

现有文件已提供良好起点，但仍缺少一项指导。现在修改核心的 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分（应在第 35 行附近）。
2. 将以下内容作为最后一条项目添加：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话启动时加载存储库指令。保存更新后的指令后，退出并恢复筛选会话，使 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录中，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话并确认当前检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并查看已更改的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认这些注释准确说明了代码。

## 总结和后续步骤

你已了解 Copilot CLI 如何读取指令文件，并将新标准应用到功能中。本课完成了以下工作：

- 查看智能体如何获取存储库指令和按路径限定范围的指令文件。
- 更新指令文件，确保遵循编码标准。
- 观察指令文件对代码的影响。

下一课将[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot - 指令文件和其他资源合集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
