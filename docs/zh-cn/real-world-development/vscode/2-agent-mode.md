---
title: "练习 2 - 使用 Copilot Agent Mode 添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行测试等操作。开发者通常需要找到所有相关文件、进行修改、运行测试、调试、发现遗漏的文件，再次修改……这样的工作可能会不断重复。

Copilot Agent Mode 正是为此而设计的。

Copilot Agent Mode 可以在 IDE 中更自主地工作。它会像开发者一样先探索现有项目结构，执行所需更新，运行测试等任务，并自动修复发现的问题。现在了解如何使用 Agent Mode 为网站添加新功能。

> [!NOTE]
> 虽然名称相似，agent mode 和云智能体针对的是两种不同体验。agent mode 在 IDE 中执行任务，支持快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和 pull request 与你交互。

在本练习中，你将了解：

- Copilot Agent Mode 如何探索项目、识别相关文件并协调更改。
- GitHub Copilot Agent Mode 如何跨 UI 和数据层实现新功能。
- 合并到代码库前，如何审查 Copilot Agent Mode 生成的更改和测试。

## 场景

随着游戏列表不断增长，你希望让用户按类别和发行方筛选。上一练习已添加发行方辅助函数；现在将使用 Copilot Agent Mode 完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

更改网站前，先探索 Tailspin Toys，了解现有功能。

该网站是一个带有开发者主题的棋盘游戏众筹平台，可列出游戏并显示其详情。网站是单个 Astro 应用，构建时将页面渲染为静态 HTML。页面在 frontmatter 中通过 Drizzle ORM 直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能使用标准 DOM API 编写的小型作用域 Astro `<script>`。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。通过以下步骤在 GitHub Codespace 中运行：

1. 返回 Codespace。上一练习结束后应已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 创建并切换到筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器启动后，会显示类似以下 URL 的提示：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否要打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），同时选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中选择终端的 localhost 链接，会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接 Codespace（当前运行网站服务器）的私有隧道。

### 探索网站

网站启动后，可以浏览现有功能：

- **主页**：显示棋盘游戏列表及其标题、图像和说明。
- **游戏详情页**：选择游戏后会打开详情页，显示标题、说明、发行方和类别等信息。

## 与 Copilot 一起查看待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始新对话。不同练习所需的模式和模型可能不同；各步骤会在相关处说明。

网站的初始实现已可正常运行，但我们想添加功能。先查看待办列表。使用模板创建项目时，会自动生成一组待办议题；让 GitHub Copilot 列出这些事项。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。Agent 智能体会自主规划并跨文件实现更改、运行终端命令并调用工具。

   ![显示 Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并非推荐某一种模型。编写本工作坊时使用了 Claude Sonnet 4.5，因此将它列入说明，希望代码建议相对一致，从而提供良好体验。不过，由于 LLM 具有概率性，你收到的建议可能与文档不同，这是正常且预期的情况。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 发送以下提示，让 Copilot 查看议题待办列表：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 以运行列出所有议题的命令。
5. 审查生成的议题列表。

留意 Copilot 还会根据它认为对用户最有用的程度，为各项议题排序。

## 查看说明文件

让智能体生成代码前，先查看用于提供工作上下文的说明文件。我们将使用[用户界面（UI）说明][ui-instructions]，它介绍了向网站添加功能的方法。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中添加功能的整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向各文件类型专用说明的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件。路径相对于说明文件所在位置，因此可以复用说明、将复杂内容拆分为较小部分，并提供示例和模板。

## 实现筛选功能

完成筛选功能至少需要对应用做三项更新：

- 在数据层 (`src/lib/`) 添加或完善剩余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页面，添加筛选 UI。

此外，合并前必须运行并通过测试。Copilot Agent Mode 可以帮你完成这些任务。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 说明文件。

   ![选择 UI 说明文件的示例屏幕截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表仍选中 **Agent**。

   ![显示 Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认模型仍选择 **Claude Sonnet 4.5**。
5. 使用以下提示，让 Copilot 根据待办议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目并查找相关文件，包括数据层辅助函数、UI 和测试。之后它会修改文件并运行测试。

   ![Copilot 正在探索项目文件的屏幕截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务复杂度和代码库规模，这可能需要几分钟。过程中编辑器可能显示 **Keep** 和 **Undo** 按钮。工作完成后，所有更改都会显示相应的 **Keep** 或 **Undo** 选项，因此不必在工作进行时立即选择。

7. 按 Copilot 的提示选择 **Continue** 运行测试。

   ![Copilot Chat 面板中询问用户是否允许运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，甚至出现测试失败。这没关系！Copilot 会在代码生成和测试之间反复迭代，直到完成任务且未检测到错误。

   ![Copilot Agent Mode 完整 Chat 会话的屏幕截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他 AI 工具生成的代码。

10. 返回运行网站的浏览器，试用新功能。
11. 确认一切正常并完成代码审查后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot Agent Mode 为 Tailspin Toys 网站添加新功能。你了解到：

- GitHub Copilot Agent Mode 如何跨 UI 和数据层实现新功能。
- Copilot Agent Mode 如何探索项目、识别相关文件并协调更改。
- 合并到代码库前，如何审查 Copilot Agent Mode 生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建 pull request。

### 额外探索练习 — 实现分页

游戏列表不断增长时，需要支持分页。使用本练习中学到的技能，提示 Copilot 为网站实现分页。编写提示和代码时可以考虑：

- 遵循现有最佳实践，包括使用已有说明文件。
- 考虑分页方式：允许用户选择每页大小，还是固定大小。
- 提示中提供必要的指导，使 Copilot 按期望实现功能。
- 可能需要与 GitHub Copilot 多次迭代、请求更改并补充上下文。这是正常的协作流程。

## 资源

- [Copilot ask、edit 和 agent 模式：用途及选择时机][choose-mode]
- [VS Code 中的 agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
