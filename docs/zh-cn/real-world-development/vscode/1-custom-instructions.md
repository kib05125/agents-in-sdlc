---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Agent mode →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解一些背景信息，就应确保它能访问这些上下文。向 Copilot 提供特定上下文的方式有多种，其中最重要的是[说明文件][instruction-files]，它们可以指导代码生成。

在本练习中，你将：

- 探索项目上下文、编码指南和文档标准如何通过存储库自定义说明及按路径限定的说明文件提供给 Copilot。
- 使用当前说明生成筛选功能的第一个数据切片（发行方辅助函数）。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。
- 再次运行相同提示，观察生成的代码如何采用新标准。
- 将说明更新和筛选功能切片提交到 `main`，供下一练习使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不完全遵循设定的某些标准。Copilot 具有概率性。本练习要观察的是更新说明后行为的*趋势变化*，而不是要求输出逐字一致。

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
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。留意指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明集拆分成可复用的小文件，并链接到示例供 Copilot 参考。（其中的路径相对于说明文件所在位置，而非存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目编码标准，但尚未要求添加代码文档。接下来将添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

遵循最佳实践，为更改创建分支：

1. 返回上一练习使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始新对话。不同练习所需的模式和模型可能不同；各步骤会在相关处说明。

要查看自定义说明的影响，先在当前说明文件不变的情况下发送提示。之后更新说明，再次发送相同提示并比较结果。

1. 关闭之前练习遗留的编辑器标签页，让 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，让 Copilot 能编辑文件。

   ![显示 Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并更新代码，通常包括辅助函数文件和测试。
6. 留意提议的辅助函数是一个类型化函数，以 `db` 客户端作为第一个参数，并返回类型化的发行方数组。这遵循 `.github/instructions/drizzle.instructions.md`（适用于 `src/lib/*.ts`）中的数据层约定。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，即使没有明确要求，也可能添加文档注释。这种情况也没关系；本练习重点在于更新说明后行为的一致性改进。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。添加编码标准可以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（大约在第 27 行附近）。留意它记录了项目编码标准，但没有代码文档规则，这也是之前生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存文件。

> [!TIP]
> 如上一课所述，说明文件可放在存储库级别（`.github/copilot-instructions.md`）提供全局指导，也可使用 `*.instructions.md` 针对语言、文件类型或任务提供说明。存储库级文件适合放置新添加的文档注释规则等项目级标准。

## 重新运行提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除上下文。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 关注正确的文件。
3. 发送与之前完全相同的提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意生成的文件现在会以类似以下注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 留意生成的函数现在包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将其应用于真实代码，后续练习会继续使用这段代码。

## 提交说明并推送分支

说明文件和存储库中的其他资源一样，也由源代码管理。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新的终端窗口。
2. 在终端中运行以下命令，确认说明更新和辅助函数更改均已存在：

   ```bash
   git status
   ```

3. 暂存并提交说明更新，以及 Copilot 为辅助函数基础工作更改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他相关更改（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 推送分支：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并 pull request

分支已推送，现在创建 pull request 并关联待办列表中的文档标准议题。Copilot 可以使用已连接到项目的 GitHub 工具查找议题、创建关联 PR 并合并。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已为工作区接入 GitHub 工具，因此无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示登录 GitHub。按照提示授权即可。后续练习会介绍这种连接如何通过 Model Context Protocol 运作。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保智能体选择器选中 **Agent**。
3. 让 Copilot 查找与更新说明文件相关的议题，并从当前分支创建 PR，说明其中包含说明更新和新的发行方辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。
5. Copilot 提示允许其代表你执行 GitHub 操作时，先审查命令，再酌情选择 **Allow**。
6. PR 创建后，使用以下提示让 Copilot 合并 PR 并将当前分支切回 main：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 提示允许其代表你执行 GitHub 操作和 shell 命令时，先审查命令，再酌情选择 **Allow**。

你已借助 GitHub Copilot 创建并合并 pull request。

## 总结和后续步骤

你探索了 Copilot 如何读取本项目说明文件中的上下文，然后使用 VS Code 中的 Copilot Chat：

- 发送代码生成提示，观察 Copilot 在*现有*说明下生成的结果。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。
- 重新运行相同提示，观察生成的代码如何采用新标准。
- 将说明更新和辅助函数基础工作提交到 `main`，供下一练习使用。

接下来，在 [agent mode][next-lesson] 中应用这些说明，让 Copilot 在整个代码库中添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot：说明文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
