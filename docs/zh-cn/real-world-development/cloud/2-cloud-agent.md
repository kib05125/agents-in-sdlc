---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债务问题，例如未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体能够自主执行更新代码和添加功能等任务。完成后，它会创建一个草稿 PR，供开发者审查。这样可以将繁琐任务交给智能体，加快开发流程，让开发者专注于更重要的工作。

你将探索 Copilot 云智能体的以下用法：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 了解范围明确的议题为何重要。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些需要解决的技术债务。最初创建网站的承包商没有留下完善的文档，事实上几乎完全没有文档。团队希望首先为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现体验。他们希望每款游戏的详情页显示相关游戏（同类别的其他游戏），让访客继续浏览。目前不要求实现完善，只需要能用于用户体验验收测试。该任务目前受阻，但其他议题优先级更高。

这类任务很容易被不断推迟，适合交由 Copilot 云智能体异步处理。开发者可以先专注其他工作，再回来审查 Copilot 的成果并确认符合预期。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发者一样在后台执行任务。可以通过多种方式启动任务，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿 pull request 来跟踪进度、设置环境并开始处理任务。无论任务正在进行还是已经完成，你都可以查看 Copilot 会话。提议的解决方案准备好审查时，Copilot 会在 pull request 中通知你。

## 明确任务范围的重要性

GitHub Copilot 并非魔法工具，不能替代思考。即使看似简单的操作，也可能包含复杂细节，因此[为 Copilot 云智能体设定任务范围时要谨慎][cloud-agent-best-practices]。将它视为 AI 结对程序员：分阶段工作、学习、试验并不断调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义说明

前面的练习介绍了自定义说明及其对 Copilot 的指导作用。如果已经完成自定义说明练习，你就了解了 `.github/copilot-instructions.md` 的作用；如果还没完成，现在可以快速查看。分配任务给 Copilot 云智能体前，先只读查看存储库中的说明文件，之后便能辨认它们的影响。

在 GitHub 网页界面或已启动的 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其留意 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将说明限定于 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 议题分配给云智能体时，留意生成的 pull request 中是否包含 TSDoc 文档注释、TypeScript 约定和文件注释头。这些内容来自上述说明文件。后续练习审查 PR 时还会再介绍。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，确保环境正常。将任务分配给 Copilot 时也是如此，它会像软件工程师一样完成工作。

云智能体使用 [GitHub Actions][github-actions] 作为工作环境。可通过创建专用设置工作流自定义环境，将其配置在 `.github/workflows/copilot-setup-steps.yml` 中，在 Copilot 开始工作前安装所需开发工具和依赖项。实验室已预先配置此工作流，以便顺利完成实验。它会为单体 Astro 应用安装 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许从存储库的“Actions”选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 仅设置步骤实际需要的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 `contents: read` 权限。如果设置步骤不克隆存储库，步骤完成后 Copilot 会自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在都是 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，确保构建和测试能访问数据。
      - name: Set up the database
        run: npm run db:setup
```

它看起来与其他 GitHub 工作流文件相似，但有几点需要注意：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理 pull request 前，GitHub Actions 会先运行此作业。
- 工作流使用 `workflow_dispatch` 触发器，因此可以在存储库的 **Actions** 选项卡中手动运行。这样可先验证工作流，而不必等待 Copilot 调用。

## 添加文档

大家都知道文档很重要，但大多数项目的文档要么过时，要么完全缺失。这类技术债务往往无人处理，降低效率，也增加维护代码库或引入新团队成员的难度。Copilot 擅长生成文档，因此很适合将此议题分配给 Copilot 云智能体。它会在后台生成所需文档，之后的练习会审查其成果。

1. 在新的浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的 pull request（PR），标题应类似 `[WIP]: Code lacks documentation`。如果列表中尚未出现新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 的待办事项列表可能需要几分钟才会出现在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定最佳解决方案。

13. 查看待办事项列表及其计划完成的任务。
14. 向下滚动 PR 时间线，查看 Copilot 开始处理议题的更新。
15. 选择 **View session** 按钮。

    ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态指示器。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码并了解当前状态、暂停思考并选择合适计划，以及编写代码。

整个过程可能需要几分钟。Copilot 云智能体的一个主要优点就是可以异步执行任务，让我们腾出时间处理其他工作。接下来利用这一点，再分配一项任务给云智能体，然后开始编写应用的新功能。

## 在游戏详情页添加相关游戏部分

如前所述，GitHub Copilot 云智能体的优势之一是可以分配并行任务：你可以专注于一组工作，而让它处理另一组工作。为设计团队添加相关游戏部分也许不需要太久，但这段时间仍可用于其他任务。现在将这项工作分配给 Copilot 云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后你会在议题的第一条评论中看到 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情符号表示它正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求！云智能体的工作方式类似软件工程师，无需持续监控；等它完成后再审查即可。现在开始创建并使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。使用云智能体可以将议题分配给 Copilot，让它异步处理技术债务、开发新功能，或将代码从一个框架迁移到另一个框架。

你探索了以下内容：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 了解范围明确的议题为何重要。
- 将议题分配给 Copilot。

云智能体正在后台工作，现在可以开始创建和使用自定义智能体。[Copilot 云智能体也可以使用 MCP 服务器][cloud-agent-mcp]，并可读取自定义说明；这些内容已在前面的模块中介绍。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
