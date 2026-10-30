---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解一些背景信息，就要确保它能访问这些上下文。[说明文件][instruction-files]可以提供这些指导，让 Copilot 不仅了解要做什么，也了解应该如何完成。

在本练习中，你将：

- 探索项目上下文、编码指南和文档标准如何通过存储库自定义说明及按路径限定的说明文件提供给 Copilot。
- 向 `.github/copilot-instructions.md` 添加一条新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 路径不同，本路径不会演示提示修改前后的对比。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示影响。稍后审查云智能体创建的 pull request 时，可以看到说明文件的作用。

## 说明文件

### 场景

Tailspin Toys 制定了一些开发实践规范和要求：

- 数据层始终需要单元测试。
- 用户界面应采用暗色模式并具有现代感。
- 应使用 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过说明文件，可确保 Copilot 获得正确的信息，并按照这些实践执行任务。

### 自定义说明

自定义说明可为 Copilot 提供上下文和偏好，帮助它了解你的编码风格和要求。你可以指定编码约定、库以及希望使用的注释类型。说明可应用于整个存储库，也可以针对特定任务或文件类型。

说明文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个说明文件。它应包含适用于大多数聊天或 CLI 请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建。例如，为 TypeScript、Astro、UI 组件或单元测试提供指导。

> [!NOTE]
> 在 IDE 中，说明文件只用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级说明和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他标准的说明文件，包括 AGENTS.md 和 CLAUDE.md，详情见[自定义说明支持][custom-instructions-support]。

### 管理说明文件的最佳实践

完整讨论如何创建说明文件超出本工作坊范围，但示例项目展示了一种有代表性的方法：

- 将 `copilot-instructions.md` 聚焦于项目级指导，例如构建内容、项目结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供专门说明。
- 使用自然语言，保持指导清晰，并提供代码应如何编写或避免如何编写的示例。

说明文件没有唯一的编写方式，正如 AI 的使用方式也不止一种。通过实践可以找到最适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应准备一套完善的说明文件。浏览本项目的说明文件时，会看到它们涵盖多种任务，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同产品界面的操作各不相同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应课程会在相关处说明。
>
> 若要查找模板或起点，请浏览 [awesome-copilot][awesome-copilot]，其中包含大量说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义说明文件

花些时间阅读此存储库附带的说明文件：一个核心 `copilot-instructions.md` 和一组用于不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中打开它们。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。留意 **Code standards** 下的 **GitHub Actions Workflows** 子部分。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并查看内容。这里有 Astro 文件、Drizzle 数据层、测试等说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意 `applyTo` 字段，它使用相对于存储库根目录的 glob 来确定说明适用范围。此处匹配任何 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 查看此项目的单元测试说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。留意指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明集拆分成可复用的小文件，并让 Copilot 参考示例。（其中的路径相对于说明文件所在位置，而非存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目编码标准，但尚未要求添加代码文档。接下来将添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来是本练习中唯一需要编辑的内容：添加一条项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。之后的练习中，云智能体会在处理你分配的议题时读取这条规则。

编辑前先创建分支（云智能体会读取议题目标分支中的说明，但仍应规范提交更改）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。添加编码标准可以改善代码建议：

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（大约在第 27 行附近）。留意它记录了项目编码标准，但没有代码文档规则，这也是之前生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所述，说明文件可放在存储库级别（`.github/copilot-instructions.md`）提供全局指导，也可使用 `*.instructions.md` 针对语言、文件类型或任务提供说明。存储库级文件适合放置新添加的文档注释规则等项目级标准。

## 提交、推送并合并说明更新

云智能体会读取议题目标分支中的说明。下一练习中将从 `main` 分配议题给 Copilot，因此必须先将说明更改合并到 `main`，云智能体才能读取。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的 pull request 并合并。下一练习中，云智能体处理议题时就会读取这些说明。

> [!TIP]
> 如果希望直接在 `main` 上操作，也可以跳过分支步骤并直接提交。本练习保留分支流程，是为了模拟真实项目的处理方式。

## 总结和后续步骤

你探索了 Copilot 如何读取本项目说明文件中的上下文，并向 `.github/copilot-instructions.md` 添加了新的存储库级标准。后续练习中可以在云智能体生成的 pull request 里看到这条标准的应用。

接下来，将[第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：说明文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
