---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "查看存储库指令，添加文档标准，并将其应用于筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要按照特定方式完成，应确保 Copilot 能访问相应指导。[指令文件][instruction-files]不仅说明希望编写什么代码，也说明代码应如何组织。现在已经构建了筛选功能，接下来查看 Copilot 使用的指令、添加一项文档标准，并将其应用于代码。

本课将：

- 了解存储库指令和按路径应用的指令文件如何传递给智能体；
- 更新指令文件，确保遵循编码标准；
- 查看指令文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码；
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值（如有可注入的 `db` 参数也应说明）；
- 可复用 Astro 组件应说明其 `Props` 接口契约，相关代码更改时应同步更新注释；
- 保留现有格式和 lint 指南。

通过指令文件，可以确保 Copilot 获得正确的信息，并按照这些实践完成任务。

## 指令文件

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解编码风格和要求。可以指定编码规范、库以及注释风格等偏好。指令既可以适用于整个存储库，也可以针对特定文件类型或任务。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。内容应是适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：针对特定语言、文件类型或任务提供指导的指令文件。

> [!NOTE]
> 其他指令格式及其支持情况会因环境而异。依赖特定格式前，请查阅[自定义指令支持参考][custom-instructions-support]。

## 浏览本项目中的自定义指令文件

启动项目时已附带一组指令文件。先查看已有内容，再进行更改，以便了解更改的效果。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，注意其中的项目简介和编码指导。此文件中的指令适用于本存储库中的每次 Copilot 交互。
4. 打开 `.github/instructions` 文件夹并浏览文件。注意其中有针对 Astro 文件、Drizzle 数据层、测试等的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它使用 glob 来确定指令适用的文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向的其他指令文件和项目现有文件。这样可以将较大的指令集拆分成更小、可复用的文件，并提供可参考的示例。

## 更新指令文件以符合团队指导

现有文件已经是很好的起点，但仍有缺失。现在修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 章节，该章节通常位于第 35 行附近。
2. 将以下内容添加为该章节的最后一项：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 应用更新后的指导

Copilot CLI 会在对话开始时加载存储库指令。保存更新后的指令后，退出并恢复筛选功能对话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 退出。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录中，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，请选择筛选功能对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，检查更改的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认它们准确说明了代码。

## 总结和下一步

你了解了 Copilot CLI 如何从指令文件获取上下文，并在功能中应用了新标准。具体来说，你：

- 查看了存储库指令和按路径应用的指令文件如何传递给智能体；
- 更新了指令文件，确保遵循编码标准；
- 查看了指令文件对代码的影响。

下一步，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保 lint 和测试以一致方式运行。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot — 指令文件集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
