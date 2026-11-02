---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务有特定做法，或者 Copilot 需要了解背景信息，就应确保它能访问这些上下文。[指令文件][instruction-files]可提供此类指导，让 Copilot 不仅了解要做什么，也了解应如何完成。

本练习将：

- 了解如何通过存储库自定义指令和按路径应用的指令文件，为 Copilot 提供项目上下文、编码规范和文档标准；
- 在 `.github/copilot-instructions.md` 中添加一条新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 系列不同，本练习不会演示添加指令前后的提示词对比。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示指令的效果。后续审查云智能体生成的拉取请求时，你会看到这些指令的影响。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 代码中应使用 TSDoc 文档注释。
- 每个文件开头都应添加注释块，说明文件的用途。

通过指令文件，可以确保 Copilot 获得正确的上下文，并按这些实践完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解编码风格和要求，从而生成更符合预期的建议和代码片段。可以指定编码规范、库以及注释风格等偏好。指令既可以适用于整个存储库，也可以针对特定文件类型或任务。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。内容应是适用于大多数聊天或 CLI 请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：针对特定任务或文件类型的指令文件。可以为 TypeScript 或 Astro 等语言，或创建 UI 组件、编写单元测试等任务提供指导。

> [!NOTE]
> 在 IDE 中，指令文件只用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级指令文件和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本研讨会的范围。示例项目展示了一种有代表性的做法，概括来说：

- 让 `copilot-instructions.md` 专注于项目级指导，例如项目说明、结构和全局编码标准。
- 使用 `*.instructions.md` 文件为文件类型（例如单元测试、Astro 组件和数据层）或特定任务提供指导。
- 使用自然语言，并让指导清晰易懂。提供代码示例，说明什么样的代码符合或不符合要求。

创建指令文件没有唯一正确的方法，使用 AI 也是如此。通过试验，逐步找到适合项目和场景的方式。

> [!TIP]
> 使用 GitHub Copilot 的项目都应有一套完善的指令文件。浏览本项目中的文件时，你会看到涵盖多种任务的指令，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同产品界面的操作各不相同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应产品的课程会在适用时介绍这些操作。
>
> 需要模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中收集了指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义指令文件

请花些时间查看本存储库附带的指令文件：一个核心 `copilot-instructions.md` 文件，以及一组适用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web 界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简要说明，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等章节。它们适用于你与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览其中内容。注意其中有针对 Astro 文件、Drizzle 数据层、测试等的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它使用相对于存储库根目录的 glob 来确定适用文件。任何符合 `**/*.test.ts` 的 TypeScript 测试文件都会匹配。
5. 留意该文件中针对本项目单元测试的具体指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并查看文件底部。注意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成更小、可复用的文件，并提供可参考的示例。路径相对于指令文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 章节记录了项目编码标准，但尚未要求添加代码文档。接下来会添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来需要编辑一次：添加一条项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。云智能体稍后处理本系列中分配的议题时会使用这条规则。

编辑前，先创建并切换到一个分支（云智能体会读取议题目标分支中的指令，但仍应整洁地提交这些编辑）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码标准，以便改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 章节，该章节通常位于第 27 行附近。注意其中记录了项目的编码标准，但还没有代码文档规则，这也是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级别（`.github/copilot-instructions.md`）创建指令文件，为整个项目提供指导；也可以使用 `*.instructions.md` 文件针对特定语言、文件类型或任务提供指导。刚刚添加的文档注释规则属于项目级标准，因此适合放在存储库级文件中。

## 提交、推送并合并指令更新

云智能体会从议题目标分支读取指令。下一练习中将把议题分配给 Copilot，而它会从 `main` 分支创建分支。因此，必须先将指令更改合并到 `main`，云智能体才能使用。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求并将其合并。下一练习将分配议题，届时云智能体会读取这些指令。

> [!TIP]
> 如果希望直接在 `main` 分支上完成本研讨会，可以跳过创建分支的步骤，并直接提交到 `main`。此处采用分支，是为了模拟真实项目中的工作方式。

## 总结和下一步

你了解了 Copilot 如何使用本项目的指令文件获取上下文，并在 `.github/copilot-instructions.md` 中添加了一条存储库级标准。接下来的练习中，可以在云智能体生成的拉取请求里看到这条标准的应用效果。

下一步，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更优 Copilot 自定义指令的 5 个提示][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
