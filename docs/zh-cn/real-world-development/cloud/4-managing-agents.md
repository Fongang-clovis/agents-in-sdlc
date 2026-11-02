---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两节练习中，你让 Copilot 云智能体处理了三项不同任务，帮助改进用户体验并添加功能。云智能体虽然可以异步自主工作，但监控这些任务仍然很重要。

可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，可以查看所有带有未关闭 PR 的智能体任务、查看已执行的操作，还可以引导正在进行的会话。

本课将：

- 浏览智能体页面，监控云智能体任务；
- 引导正在进行的会话，要求添加更多功能。

## 场景

将高对比度模式任务分配给智能体后，团队意识到也应该添加浅色模式。既然智能体已经在更新网站样式并添加切换功能，就适合同时加入浅色模式。你需要引导智能体，确保它同时实现浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看分配给 Copilot 云智能体的任务状态：

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。列表中应能看到你分配给 Copilot 的任务，包括：
   - 更新代码库文档；
   - 生成用于修改游戏的 API；
   - 为网站添加高对比度模式。
3. 选择一项正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码；
   - 为 Copilot 创建环境；
   - 设置 MCP 服务器；
   - 执行完成指定任务所需的其他步骤。

> [!NOTE]
> 具体显示的步骤会因 Copilot 当前的工作状态和采取的方案而异。

4. 另请查看右侧显示的 PR 面板，以便监控 PR 和更改的文件。

## 引导云智能体

现在，向正在实现高对比度模式的 Copilot 请求添加浅色模式切换开关。

1. 选择与添加高对比度模式有关的会话。会话标题取决于 Copilot 使用的名称和当前工作状态。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到设置完成并开始处理任务。开始工作后，会出现类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示词：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![任务控制中心页面中突出显示 Steer active session while Copilot is working 对话框的云智能体任务截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示词。
5. 注意 Copilot 会确认收到提示词，并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务！它会先完成发送消息时正在执行的具体步骤，然后将新请求加入工作流程。

这可能需要几分钟，正好可以暂停一下，回顾目前学到和体验过的内容。

## 总结和下一步

本课介绍了 Copilot 智能体页面，这是监控和引导 GitHub Copilot 云智能体任务的中心位置。在这里可以跟踪所有正在进行和已完成的任务、审查工作，也可以重定向正在运行的任务来调整范围或补充指导。

本课介绍了以下概念：

- 使用任务控制中心和智能体页面监控云智能体任务；
- 重定向正在运行的会话，要求添加更多功能。

Copilot 正在完成无障碍功能，接下来将[迭代 Copilot 创建的拉取请求][next-lesson]。

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
