---
title: "课程 4 - 使用自定义说明引导 Copilot"
description: "探索存储库说明、添加文档标准，并将其应用于筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，就应让 Copilot 能够获取相应指导。[说明文件][instruction-files]不仅描述你需要什么代码，也说明代码应如何组织。现在你已经构建了筛选功能，接下来将探索 Copilot 使用的说明、添加文档标准，并将其应用于代码。

在本课程中，你将：

- 了解存储库说明和按路径限定的说明文件如何提供给智能体。
- 更新说明文件，确保遵循编码标准。
- 观察说明文件对代码的影响。

## 场景

Tailspin Toys 制定了一系列开发规范和要求：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 记录用途、参数和返回值；如果有可注入的 `db` 参数，也应予以说明。
- 可复用 Astro 组件应记录其 `Props` 接口；相关代码更改时，注释也应保持更新。
- 应保留现有格式和 lint 指南。

通过说明文件，可以确保 Copilot 获得正确信息，并按照这些实践执行任务。

## 说明文件

自定义说明可以向 Copilot 提供上下文和偏好，帮助它了解你的编码风格和要求。你可以指定编码约定、库以及希望加入的注释类型。说明可应用于整个存储库，也可以针对特定任务或文件类型。

说明文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个说明文件。它应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导的说明文件。

> [!NOTE]
> 其他说明格式和支持情况因环境而异。依赖特定格式前，请查看[自定义说明支持参考][custom-instructions-support]。

## 探索项目中的自定义说明文件

项目模板已包含一些说明文件。先探索现有文件，再进行更改，以便了解更改的影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，留意项目简介和编码指导。这些说明适用于 Copilot 在此存储库中的每次交互。
4. 打开 `.github/instructions` 文件夹并查看其中内容。这里有 Astro 文件、Drizzle 数据层、测试等说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意开头的 `applyTo` 字段，它设置了说明适用文件的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向其他说明文件和项目现有文件的链接。较大的说明集可以拆分成可复用的小文件，并链接到示例供 Copilot 参考。

## 更新说明文件

Copilot CLI 会在对话开始时加载存储库说明。保存更新后，退出并恢复筛选对话，使 Copilot 在保留功能上下文的同时加载新说明。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认检出的是 `game-filters-cli` 分支。
3. `.github/copilot-instructions.md` 中应有 **Code formatting guidance** 部分。将以下内容添加为该部分最后一条：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

4. 保存文件。
5. 让 Copilot 根据更新后的指导修改已生成的代码：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

6. 输入 `/diff`，检查更改的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认内容准确。

## 总结和后续步骤

你探索了 Copilot 如何读取说明文件中的上下文，并将新的标准应用于功能。在本课程中，你：

- 了解存储库说明和按路径限定的说明文件如何提供给智能体。
- 更新说明文件，确保遵循编码标准。
- 观察了说明文件对代码的影响。

接下来，你将[自定义并运行可复用的质量检查技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot：说明文件和其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
