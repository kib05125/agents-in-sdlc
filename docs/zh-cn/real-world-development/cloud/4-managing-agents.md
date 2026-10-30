---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前两项练习中，你让 Copilot 云智能体处理了三个不同任务，以改进用户体验并添加功能。云智能体虽然会异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个控制中心，可以查看所有创建了未关闭 pull request（PR）的智能体任务，了解它执行的操作，还可以引导正在运行的会话。

在本课程中，你将：

- 浏览智能体页面，监控云智能体任务。
- 在任务进行期间引导会话，要求添加其他功能。

## 场景

团队在分配高对比度模式任务后，意识到也可以同时添加浅色模式。既然正在更新网站样式并添加切换功能，就适合一并实现此功能。你将引导智能体，同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看分配给 Copilot 云智能体的所有任务的当前状态：

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。应能看到你分配给 Copilot 的任务，包括：
   - 为代码库补充文档。
   - 生成修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 为 Copilot 创建工作环境。
   - 设置 MCP 服务器。
   - 执行完成任务所需的各种步骤。

> [!NOTE]
> 列出的具体步骤会根据 Copilot 的工作状态和采用的方法而不同。

4. 还可查看右侧的 pull request（PR）面板，监控 PR 和更改文件。

## 引导云智能体

现在你已了解正在运行的任务，让 Copilot 在实现高对比度模式时也添加浅色模式切换开关。

1. 选择正在添加高对比度模式的会话。具体标题取决于 Copilot 使用的名称和当前工作状态。

   ![控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到设置完成并开始实际工作。出现类似下方的消息时，就表示已开始。
3. 在 **Steer active session while Copilot is working** 对话框中，添加以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![智能体页面中的云智能体任务截图，其中突出显示了引导正在运行的会话对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认提示并将其纳入工作流程。

## 让 Copilot 继续工作

和之前一样，Copilot 会开始处理更新后的任务！它会先完成发送消息时正在执行的步骤，再将新请求加入流程。

这通常需要几分钟，可以利用这段时间回顾目前学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，这是监控和引导 GitHub Copilot 云智能体任务的中心位置。在此控制中心中，你可以跟踪所有正在进行和已完成的任务、审查工作，也可以在任务进行中调整范围或提供更多指导。

你探索了以下内容：

- 使用控制中心和智能体页面监控云智能体任务。
- 引导正在进行的会话，要求添加其他功能。

Copilot 正在完成无障碍功能。接下来，你将[迭代 Copilot 创建的 pull request][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
