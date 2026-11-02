---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

使用 GitHub Copilot 代理模式时，它会自主探索代码库、提出更改、编辑文件并运行命令。由于这些工作在本机实时执行，VS Code 中的 Copilot Chat 会显示每次工具调用和文件编辑。可以内联审查每个差异、接受或拒绝单项更改，并通过后续提示词引导对话、完善或扩展工作，全程无需离开编辑器。

本练习将：

- 实时监控无障碍智能体的工作；
- 审查并接受建议的文件编辑；
- 在会话进行中引导智能体，添加浅色模式切换开关。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义智能体正在实现高对比度模式。工作进行期间，设计团队又提出一项请求：他们还希望为觉得默认主题太暗的用户添加浅色模式切换开关。无需从头开始新对话，可以引导现有会话扩展工作。

## 监控 Copilot 的进度

Copilot Chat 视图会显示智能体执行的每个步骤，包括探索的文件、运行的命令和提出的编辑。每项文件编辑都会以内联差异的形式显示，并提供 **Keep** 和 **Undo** 控件。

1. 返回练习 4 中使用的 Copilot Chat 视图。
2. 智能体工作时，滚动聊天内容查看每次工具调用。可以看到文件读取、文件编辑，以及它请求执行的终端命令。
3. 智能体提出文件编辑时，差异会以内联方式显示：
   - 选择 **Keep** 接受更改；
   - 选择 **Undo** 撤销更改，让智能体重新尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看智能体在本次会话中修改的完整文件列表。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你认可的命令。

5. 等待智能体完成高对比度模式。它应该会显示总结所做更改的最终消息。

## 引导智能体添加浅色模式

高对比度模式已实现，现在继续在同一对话中添加浅色模式切换开关。由于继续使用同一聊天会话，智能体会保留之前工作的上下文，包括修改的组件、使用的样式方案和本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示词：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 智能体会继续现有工作，检查已做更改并提出浅色模式的新增内容。
3. 与高对比度模式一样，逐项审查新增编辑，并对每个差异选择 **Keep** 或 **Undo**。
4. 如果智能体的实现方向不符合预期，例如试图以不喜欢的方式重构切换 UI，可以选择聊天输入框旁的 **Stop** 按钮停止，再发送说明性后续消息。

> [!TIP]
> 在正在进行的会话中引导智能体，比从头开始更快。可以使用后续提示词要求调整（“简化 CSS，改用 CSS 变量，不要重复选择器”）、纠正方向（“不要添加第三个开关，将浅色模式集成到现有设置面板”）或扩展范围（“为两个开关添加键盘快捷键”）。

## 审查工作集

提交工作前，快速检查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审查所有已更改文件。应该会看到 Astro 组件、样式和相关测试的更新。
3. 打开几个已更改文件并查看差异。确认自定义智能体要求的无障碍模式已应用，包括 ARIA 属性、键盘导航、语义 HTML 和本地存储持久化。
4. 满意后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结和下一步

你使用 Copilot Chat 代理模式监控并引导正在进行的会话：查看每项文件编辑、接受或拒绝更改，并通过后续提示词扩展工作，同时保留上下文。

本课中，你：

- 实时监控智能体工作；
- 接受或拒绝单项文件编辑；
- 通过后续提示词在会话进行中引导智能体。

下一课将[发布无障碍功能并审查更改][next-lesson]，包括本实验中本地自定义智能体生成的内容。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用代理模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
