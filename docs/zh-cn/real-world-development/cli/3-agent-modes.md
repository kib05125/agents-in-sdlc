---
title: "课程 3 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案、使用 Autopilot 根据议题构建筛选功能，并在 Interactive 模式下审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们已经为项目添加了一个小功能，但更大的更改需要更完善的流程。GitHub Copilot CLI 可以配合组织现有的工作流，帮助我们以正确的方式构建正确的功能。本课程将开启一系列课程，介绍典型的智能体驱动开发流程：从议题出发生成新功能，确保代码有效、功能符合预期，最终成功合并到项目中。

> [!NOTE]
> 后续功能工作将沿用同一对话和分支。通常，不同类型的文件会分别放在不同分支或 PR 中；本工作坊为了聚焦核心概念，采用了简化流程。

本课程将带你：

- 从 GitHub 议题开始新的 Copilot 对话。
- 在 Plan 模式中确定需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续实现此功能时，你还将更新存储库说明、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体，并创建功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，访客需要按类别和发行方筛选游戏。待办议题描述了该功能，但组合类别等细节需要在编写代码前确定。你将使用 Plan 模式解决这些问题，然后授权 Autopilot 执行范围明确的实现。

## 背景

将 AI 编码智能体引入开发流程，并不会改变软件开发的基本原则。事实上，这些原则变得更重要了。大多数开发者的流程大致如下：

1. 从描述待办事项的已创建议题开始。
2. 制定实现计划。
3. 编写并审查代码。
4. 运行测试验证代码。
5. 手动验证新功能。
6. 创建 pull request（PR）。
7. 代码经过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程因团队和组织而异，但大多数流程都与上述步骤类似。

遵循这种标准流程，可以确保 AI 生成的代码满足需求，并接受与手写代码相同的审查。

## 对话模式

**对话模式**决定智能体拥有多大自主权。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：你与智能体协作。智能体提出更改，并等待你的输入后再继续。
- **Plan**：智能体先创建计划，且不能编辑项目文件。
- **Autopilot**：智能体自主编写代码、运行测试并迭代，直至任务完成。

先使用 Plan 模式审查计划，再使用 Autopilot 实现。

## 从议题开始

开始筛选功能前，返回 Codespace，确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题加入提示，并返回 **Session** 选项卡。

留意提示现在以 `#7`（或类似编号）开头。`#` 可将 GitHub 上的议题或 pull request（PR）加入上下文。

## 规划筛选功能

规划能让你在交由 Copilot 实施前，确定功能或任务的实现方案。对于复杂任务，最好花些时间规划。现在切换到 Plan 模式，让 Copilot 创建计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会探索项目并确定合适的实现方式，逐步生成计划。

3. Copilot 可能会询问筛选功能应如何工作。根据自己的偏好回答，没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动审查计划。
6. 如果计划中的任何部分不符合你的决定，请让 Copilot 修改。

## 批准 Autopilot

计划已经编写并审查，现在可以实现了。让 Copilot 使用 Autopilot 执行计划。

Autopilot 会让 Copilot 持续迭代，直到它认为问题已解决。

1. 选择 **Accept plan and build on autopilot (recommended)**，或你所用版本中类似的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 按计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决环境问题，再判断检查是否完成。

## 审查并验证实现

生成的代码与其他代码一样，合并前必须审查。现在检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能实现和测试。
3. 对照议题和规划时做出的决定检查结果。
4. 审查代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 检查项目运行的检查结果；如有失败，请让 Copilot 解决。

## 试用新功能

代码看起来不错，但实际运行情况如何？像之前一样启动应用，通过 Codespaces 转发端口打开网站。

1. 请求 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行方筛选，以及计划中确定的组合方式。
4. 确认重置和无结果时的行为符合议题和你的决定。
5. 返回 Codespace，让 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你使用不同的对话模式构建并审查了功能。在本课程中，你：

- 从 GitHub 议题开始了新的 Copilot 对话。
- 在 Plan 模式中确定需求。
- 使用 Autopilot 模式实现新功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，通过[使用自定义说明][next-lesson]深入了解代码生成，确保生成的代码遵循已记录的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
