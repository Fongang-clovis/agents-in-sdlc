---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本系列练习中，你与 GitHub Copilot 协作完成了多项任务，以改进用户体验并添加功能：为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现包括高对比度和浅色模式切换在内的无障碍功能。现在来检查代码更改，必要时向 Copilot 提供反馈，改进其工作。

### 场景

如前面多次强调的，生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循正常的 DevOps 流程。因此，在为团队其他成员开启审查之前，先审查 GitHub Copilot 为文档、相关游戏功能和无障碍功能生成的建议。

## 安全与 GitHub Copilot 云智能体

由于 Copilot 云智能体异步且无人监督地执行任务，因此它受到一些安全限制：

- Copilot 对存储库只有读取权限，对代码的写入权限仅限于它使用的分支。
- 云智能体在 GitHub Actions 中运行，并创建独立的临时环境进行工作。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- [默认情况下，外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也包括在内。

## 审查生成的文档

先查看 GitHub Copilot 云智能体生成的第一个 PR：为代码添加文档。通过 GitHub.com 上的常规 PR 界面完成审查。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认无法访问许多外部资源，包括外部 MCP 服务器。可以根据需要[自定义或禁用云智能体防火墙][agent-firewall]。

1. 在 github.com 上打开存储库。
2. 选择 **Pull Requests**，打开 PR 列表。
3. 打开标题类似 **Add missing documentation** 的 PR。

> [!NOTE]
> 如果 Copilot 还在处理任务，PR 标题会带有 **[WIP]** 标记。此时请等待它完成，可能需要几分钟。等待期间可以休息一下，或回顾已学内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新增的 TSDoc 文档注释和其他文档。具体更改可能会有所不同。

    浏览更改时，留意 TSDoc 文档注释、TypeScript 规范和文件注释头。这些要求来自练习 2 开始时查看的自定义指令文件。

6. 审查完成且结果符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. 工作流会在 PR 检查部分排队运行。如果一切正常，单个 Astro 应用的项目检查就会通过。这可能需要几分钟。

## 请求 GitHub Copilot 更改

与 Copilot 协作并非单向过程。你可以像对待团队成员一样，在 PR 评论或代码行内评论中标记 Copilot。Copilot 会读取评论并启动新会话来处理请求。由于结果并非确定，因此无法提供固定的请求文本。可以要求 Copilot：

- 在每个代码文件顶部添加简要说明文件用途的注释头；
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释；
- 创建 README，介绍 Astro 应用程序结构。

1. 添加评论并标记 **@copilot**，请求更新生成的文档。可以使用上面的示例，或提出其他代码文档改进建议。
2. 选择 **View Session**，观察 Copilot 执行更新。注意 Copilot 会启动新会话来处理更改。
3. 选择 **Back to pull request** 返回 PR。

    ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中会出现新的提交。
5. 选择 **Files changed** 选项卡审查更改。

可以继续迭代，直到满意为止。完成后，可以将 PR 从草稿转换为准备审查状态，然后合并到 main 分支。

![将 PR 转为准备审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在查看 Copilot 为解决“在游戏详情页显示相关游戏”议题创建的 PR。

1. 在 GitHub.com 上打开存储库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡审查生成的代码。
5. 审查完成且结果符合预期后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准的提示。
7. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流会在 PR 检查部分排队运行。如果一切正常，单个 Astro 应用的项目检查就会通过。这可能需要几分钟。
9. **可选：** 可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已经构建了相关游戏功能！与之前一样，可以继续迭代并请求更新。例如，可以调整显示的相关游戏数量，或要求添加注释头和 TSDoc 文档注释（请记住，分配此任务时，你还没有更新自定义指令）。与之前一样，在 **Conversation** 选项卡中添加评论，Copilot 会据此启动新会话。

## 审查无障碍功能

最后，审查通过无障碍自定义智能体实现的功能。此 PR 应同时包含练习 3 中要求的高对比度模式，以及练习 4 在任务控制中心追加请求的浅色模式。

1. 在 GitHub.com 上打开存储库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 还在处理任务，PR 标题会带有 **[WIP]** 标记。请等待它完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡审查生成的代码。
5. 查看实现，特别留意：
   - 用于切换模式的 UI 组件；
   - 用于保存用户偏好的本地存储；
   - 高对比度和浅色模式的 CSS 或样式更改；
   - 无障碍属性（ARIA 标签、键盘导航等）；
   - 管理模式切换的 JavaScript/TypeScript 代码。
6. 审查完成且结果符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流会在 PR 检查部分排队运行。如果一切正常，单个 Astro 应用的项目检查就会通过。这可能需要几分钟。
10. **可选：** 可以在 Codespace 中切换到此分支，手动测试无障碍功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换，并确认页面重新加载后仍保留所选模式。

注意，无障碍自定义智能体如何帮助 Copilot 按照无障碍最佳实践实现这些功能。如果发现无障碍问题或可改进之处，可以像审查其他 PR 一样，在评论中标记 **@copilot** 并请求更新。

## 可选练习 — 继续委派工作

云智能体最适合处理真实的待办事项，让你能同时处理其他任务。可以创建几个新议题并分配给 Copilot，培养这种工作方式。示例：

- 在游戏详情页创建支持者兴趣表单；
- 为游戏列表页添加分页；
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体系列课程。在这些课程中，你：

- **检查了存储库附带的自定义指令文件**，以便之后观察它们对云智能体输出的影响；
- **将议题分配给 Copilot 云智能体**，观察它设置环境、制定计划并异步执行任务；
- **创建并使用自定义智能体**，添加高对比度和浅色模式切换；
- **使用 Copilot 智能体页面作为任务控制中心**，监控并引导无障碍会话；
- **审查并迭代云智能体的拉取请求**，使用 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

你已完成云智能体系列课程。想继续探索的话，可以学习其他系列：

- 🖥️ **[VS Code 系列](../../vscode/)** — 在 IDE 中直接体验 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 系列](../../cli/)** — 在终端中交付两项经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

也可以在自己的存储库中尝试以下做法：

- 将重构或测试覆盖率议题分配给云智能体；
- 为其他领域创建自定义智能体；
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
