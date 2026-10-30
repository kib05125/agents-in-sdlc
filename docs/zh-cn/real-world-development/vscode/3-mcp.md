---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：Agent mode][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用 agent mode 构建了筛选功能。创建 pull request 前，应确认它在浏览器中正常工作。无需亲自点击浏览应用，你可以连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器测试功能，然后发布分支并创建 PR。

在本练习中，你将：

- 了解 Model Context Protocol（MCP）以及 MCP 服务器如何为 Copilot 扩展工具。
- 将 Playwright MCP 服务器添加到工作区。
- 让 Copilot 使用它在浏览器中手动测试筛选功能。
- 发布分支，并为筛选功能创建 pull request。

## 什么是 Model Context Protocol（MCP）？

当 agent mode 可以访问编辑器之外的资源时，功能会大幅增强。Copilot 通过 Model Context Protocol（MCP）访问这些资源。MCP 是智能体与外部工具和服务通信的标准方式。

![展示 agent mode 内部工作方式以及它如何与上下文、LLM 和工具交互的示意图，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol（MCP）](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)为 AI 智能体与外部工具和服务通信提供了途径。使用 MCP，AI 智能体可以实时访问最新信息（通过资源）并代表你执行操作（通过工具）。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当智能体与外部工具和服务之间的桥梁，负责管理智能体与外部工具（例如现有 API 或 NPM 包等本地工具）之间的通信。每个 MCP 服务器代表智能体可访问的一组工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供用于管理 GitHub 存储库的 API，智能体可以创建存储库、更新存储库，以及管理议题和 pull request。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力，智能体可以访问网页、填写表单和选择按钮。

还有许多其他 MCP 服务器可提供不同工具和资源。GitHub 提供了 [MCP 注册表](https://github.com/mcp)，帮助用户发现并参与生态系统。

> [!CAUTION]
> 从安全角度看，应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前请仔细审查源代码、验证发布者并考虑安全影响。只使用信任的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件配置此 VS Code 工作区可用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 应该能看到已配置的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

项目模板已包含此 `github` 条目，因此上一练习中 Copilot 才能读取待办议题。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需本地安装。VS Code 通过 HTTP 连接，Copilot 首次使用其中某个工具时需要通过 GitHub 身份验证。

## 添加 Playwright MCP 服务器

现在添加第二台服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的浏览器，可用于测试功能。

1. 在 `.vscode/mcp.json` 中，在 `github` 旁边添加 `playwright` 条目，使文件内容如下：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标记让 Playwright 在后台运行浏览器，不显示窗口。Codespace 没有可显示窗口的桌面，因此必须使用此选项。

> [!NOTE]
> Tailspin Toys 项目已在端到端测试中使用 Playwright，通常已安装 Playwright 所需的浏览器。如果 Copilot 报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器：Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择它以立即启动服务器并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请审查配置并选择信任，使服务器可以启动。

手动启动是可选的。如果跳过，VS Code 会在下一步 Copilot 首次需要此工具时启动服务器，并询问是否信任。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可驱动的真实浏览器。智能体可以打开页面、导航、应用筛选并读取结果，然后总结观察内容。无需离开对话，就能快速确认功能是否按预期运行。

Playwright MCP 服务器在底层使用页面的[无障碍树][playwright-mcp-server]，而不是截图。因此，智能体会基于带标签的结构化元素（按钮、链接、列表项）进行推理，与辅助技术的工作方式相同；快速的功能检查也能顺便进行基础无障碍检查。

连接服务器并启动应用后，使用以下提示让 Copilot 测试刚完成的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器，逐步执行检查并报告结果。根据议题验收标准审查其报告；如果发现异常，可继续提问，或让 Copilot 修复代码后再创建 pull request。

> [!NOTE]
> 测试前，应用必须运行在 `http://localhost:4321`。如果开发服务器已停止，请先重新启动。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium` 后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建 pull request

确认功能正常后，就可以创建 pull request（PR）供团队审查了。首先发布 `filtering-vscode` 分支。

1. 在 Codespace 中打开 **Source Control** 面板，审查 Copilot 所做更改。
2. 选择 **+** 图标暂存更改。
3. 选择 **Sparkle** 按钮生成提交消息。

   ![显示更改内容的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建 pull request

可以通过 github.com 或 GitHub 命令行接口（CLI）等方式创建 pull request。既然正在使用 GitHub Copilot，就让它来创建 PR。它可以找到相关议题并将其关联到 PR。

1. 打开 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 确认智能体下拉列表选中 **Agent**，这样 Copilot 才能使用 GitHub 工具。
3. 使用以下提示让 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 按需选择 **Continue**，允许 Copilot 收集信息并执行操作。首次使用 GitHub 工具时，可能会提示通过 GitHub 身份验证。按照提示授权。
5. 留意 Copilot 如何搜索议题、找到相关议题并创建 PR。
6. 选择 Copilot 生成的链接审查 pull request，但**暂时不要合并**。

## 总结和后续步骤

恭喜！在本练习中，你：

- 了解了 Model Context Protocol（MCP）以及 MCP 服务器如何为 Copilot 扩展工具。
- 将 Playwright MCP 服务器添加到工作区。
- 在交付筛选功能前，使用它在浏览器中手动测试了功能。
- 发布分支并创建了筛选功能 pull request。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义智能体][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 是什么，为什么大家都在谈论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
