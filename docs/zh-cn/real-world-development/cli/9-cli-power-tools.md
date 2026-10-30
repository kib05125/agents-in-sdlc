---
title: "课程 9 - GitHub Copilot CLI 斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话和可选云端委派的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

和其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多斜杠命令。这些命令可访问高级功能、后台信息和其他配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令，现在来探索其他实用命令。

在本课程中，你将：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文。
- 使用 `/model` 探索可用模型。
- 了解 `/share` 如何导出或共享会话。
- 探索用于并行工作、工作树以及委派给云智能体的可选命令。

## 场景

核心 CLI 工作流已完成。现在来了解其他能力：管理上下文、切换模型、共享会话，以及可选地将工作委派给 [Copilot 云智能体][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理大型或复杂任务时，可能会接近模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话；你也可以使用斜杠命令检查或手动压缩上下文。

1. 返回 Codespace；如果 Copilot CLI 尚未打开，则从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 留意模型、当前 token 用量，以及系统说明、工具、消息和可用空间分别占用的上下文。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> 上下文窗口接近填满时，Copilot CLI 会自动压缩上下文。想自行决定压缩时机时，可使用 `/compact`。如果要切换到无关任务并开始全新对话，请使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，开发者的偏好也不同。Copilot CLI 可以列出模型并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 探索可用模型和用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

与团队协作并分享经验，有助于大家更好地使用 AI 工具。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布 GitHub gist。

1. 输入 `/help`，查看你所用版本中 `/share` 的选项。
2. 若要共享会话，请选择适合的目标，例如 `/share file`，导出本地 Markdown 文件。
3. 分享或发布之前，先审查导出内容。会话导出可能包含提示、回答和项目详细信息。

发布链接或 gist 是可选操作。不要发布团队无意共享的存储库内容或对话内容。

## 可选：扩展并行工作或委派任务

核心工作坊已结束。Copilot CLI 还提供了一些适用于大型任务的命令：

- `/fleet` 可将彼此独立的子任务分配给子智能体并行执行。
- `/worktree` 可创建隔离的 Git 工作树，用于处理独立任务。
- `/delegate` 可将任务发送给 Copilot 云智能体，由其异步处理并可能创建 pull request。

这些命令是可选的，因为它们可能创建额外工作树或远程工作。尝试前，请开始一个范围明确的新任务，并按照常规工作流审查结果。若要深入了解异步智能体工作，可继续学习[云智能体工作坊][cloud-workshop]。

## 总结和后续步骤

使用 Copilot CLI 的斜杠命令，可以进行配置、共享会话并查看后台状态。在本课程中，你：

- 使用 `/context` 和 `/compact` 探索 Copilot 如何管理对话上下文。
- 使用 `/model` 探索可用模型。
- 了解 `/share` 如何导出或共享会话。
- 探索了用于并行工作、工作树和云端委派的可选命令。

Copilot CLI 还有更多斜杠命令值得探索！接下来通过[回顾所学内容][next-lesson]并了解后续步骤，为本系列课程收尾。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云智能体][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
