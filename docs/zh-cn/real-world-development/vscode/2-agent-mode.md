---
title: "练习 2 - 使用 Copilot 代理模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行测试等操作。开发人员往往需要找出所有相关文件、完成更改、运行测试、调试、发现遗漏的文件并再次修改，工作往往会不断重复。

Copilot 代理模式可以帮助处理这些工作。

Copilot 代理模式能在 IDE 中更自主地工作。它会像开发人员一样探索项目结构、执行必要更新、运行测试等任务，并自动修复发现的问题。接下来了解如何使用代理模式为网站添加新功能。

> [!NOTE]
> 代理模式和云智能体的名称相似，但适用于不同的工作方式。代理模式在 IDE 中执行任务，便于快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将了解：

- Copilot 代理模式如何探索项目、识别相关文件并协调更改；
- GitHub Copilot 代理模式如何跨 UI 和数据层实现新功能；
- 合并前如何审查 Copilot 代理模式生成的更改和测试。

## 场景

游戏列表不断增长，因此希望允许用户按类别和发行商筛选。上一练习已添加发行商辅助函数，接下来使用 Copilot 代理模式完成其余数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行更改之前，先探索 Tailspin Toys 网站，了解它当前的功能。

该网站是一个以开发者为主题的桌游众筹平台，可列出游戏并显示详细信息。它是一个单独的 Astro 应用程序，在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能使用基于标准 DOM API 的轻量 Astro `<script>`。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。通过以下步骤在 GitHub Codespace 中运行：

1. 返回 Codespace。上一练习完成后，当前应位于 `main` 分支。
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

   Astro 开发服务器就绪后，会显示类似以下的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出提示，询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），并选择终端中的 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中选择终端里的 localhost 链接时，系统会自动将其转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到 Codespace 的私有通道，网站服务器正在其中运行。

### 探索网站

网站运行后，可以探索其功能：

- **主页**：显示桌游列表，包括标题、图片和描述；
- **游戏详情页**：选择游戏后会打开详情页，显示标题、描述、发行商和类别等信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型选择因练习而异；每个步骤会在相关处说明。

网站的初始实现已能正常运行，但还可以添加新功能。先查看待办事项。根据模板设置项目时，系统已自动创建一组议题。让 GitHub Copilot 显示这些议题。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。**Agent** 智能体可以自主规划并跨文件实现更改、运行终端命令并调用工具。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写研讨会时使用了 Claude Sonnet 4.5，因此在说明中列出了它，希望代码建议相对一致，以提供良好体验。不过，由于 LLM 具有概率性，收到的建议可能与课程示例不同，这是正常且预期的情况。

> [!NOTE]
> LLM 具有概率性，因此 Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示词，询问待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看生成的议题列表。

注意 Copilot 会根据对用户最有用的程度为议题排序。

## 查看指令文件

让智能体生成代码之前，先查看用于向 Copilot 提供上下文的指令文件。本练习将使用 [用户界面（UI）指令文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中介绍了为网站添加功能的方法。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看添加功能的整体指导，包括：
   - 架构概述；
   - 组件设计、可测试性和无障碍原则；
   - 指向不同文件类型专用指令的链接，包括 Astro、Tailwind CSS 和 Drizzle。

> [!TIP]
> 指令文件可以引用其他指令文件和项目中的文件。路径相对于指令文件所在位置。这样可以复用内容，将复杂指令拆分为更小、更易管理的部分，并提供示例和模板。

## 实现筛选功能

要完成筛选功能，至少需要对应用进行三项更改：

- 在数据层（`src/lib/`）中添加或完善其余筛选逻辑；
- 添加或更新筛选行为测试；
- 更新游戏列表页面，加入筛选 UI。

此外，合并前还需要运行并通过测试。Copilot 代理模式可以完成这些任务。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 `ui` 指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图中仍选中 **Agent**。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 确认当前模型仍为 **Claude Sonnet 4.5**。
5. 使用以下提示词，让 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何探索项目并查找相关文件，包括数据层辅助函数、UI 和测试，然后修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务复杂度和代码库大小，可能需要几分钟。过程中编辑器可能会显示 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示相应的 **Keep** 或 **Undo** 选项；工作尚未完成时无需选择。

7. 按 Copilot 的提示选择 **Continue**，运行测试。

   ![Copilot Chat 面板中确认是否运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会有暂停，甚至出现测试失败，这没有问题。Copilot 会在生成代码和运行测试之间迭代，直至任务完成且未发现错误。

   ![Copilot 代理模式完整 Chat 会话截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 请始终审查 Copilot 或其他 AI 工具生成的代码。

10. 返回运行着网站的浏览器，探索新功能。
11. 确认一切正常并完成代码审查后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和下一步

恭喜你！本练习介绍了如何使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加新功能。你了解了：

- GitHub Copilot 代理模式如何跨 UI 和数据层实现新功能；
- Copilot 代理模式如何探索项目、识别相关文件并协调更改；
- 合并前如何审查 Copilot 代理模式生成的更改和测试。

接下来使用 [Playwright MCP 服务器][next-lesson]测试功能，并为其创建拉取请求。

### 额外探索练习 – 实现分页

随着游戏列表不断增长，网站将需要分页功能。可以运用本练习中学到的技能，提示 Copilot 为网站添加分页。考虑以下事项：

- 遵循现有最佳实践，包括使用已有指令文件；
- 考虑分页方式：允许用户选择每页大小，还是使用固定值；
- 编写提示词时，提供足够指导，让 Copilot 按预期实现功能；
- 可能需要与 GitHub Copilot 迭代，多次提出更改请求并提供上下文。这是正常的协作流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能和使用时机][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
