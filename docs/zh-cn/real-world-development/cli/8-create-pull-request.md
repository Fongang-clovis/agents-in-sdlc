---
title: "第 8 课 - 创建并合并功能 PR"
description: "一起审查筛选功能、指令、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、指令更新、技能更新、质量保证（QA）配置和测试都已保存在同一分支。现在一起审查这些更改并创建拉取请求。星级评分 PR 是手动合并的；这次将让 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常应将功能、指令更新、技能更新和 QA 智能体拆分到几个独立的 PR 中。为简化本研讨会，所有筛选和质量工作都保留在同一个对话和分支中，并通过这个 PR 一起提交。

本课将：

- 了解 Agent Merge 及其如何自动化合并流程；
- 检查完整的功能更改和验证证据；
- 创建筛选功能 PR；
- 审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选功能工作流中，你使用 Copilot 规划、实现并验证了功能。现在 Tailspin Toys 希望自动处理剩余 PR 工作，同时由开发人员保留合并授权。

## 认识 Agent Merge

**Agent Merge** 会自动处理合并拉取请求所需的剩余工作。启用后，Copilot 会解决阻止 PR 合并的问题，例如修复失败的持续集成（CI）检查、回复审查评论，以及在需要时进行变基；如果存储库允许，还会启用 GitHub 自动合并。

到目前为止，你都是自行选择 **Merge pull request**。Agent Merge 可以代为处理这一步。启用之前，仍应审查工作并确认已准备好合并。

## 使用 Agent Merge 管理 PR

代码编写完成后，一起审查代码、创建 PR，并让 Agent Merge 管理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 输入 `/pr create` 创建新的 PR。
5. 输入 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程。所有检查成功后，它会执行合并。
7. 确认 Copilot 显示类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、存储库设置或权限要求。如果合并受阻，请查看报告的原因；在存储库允许的情况下，完成审查后手动合并。

## 总结和下一步

你已经自动化了开发流程的多个部分，包括生成代码、测试和验证代码，以及处理拉取请求。本课中，你：

- 了解了 Agent Merge 及其如何自动化合并流程；
- 检查了完整的功能更改和验证证据；
- 创建了筛选功能 PR；
- 仅在审查完成后启用了 Agent Merge，并确认 PR 已合并。

接下来，[探索更多 Copilot CLI 斜杠命令][next-lesson]，管理上下文、模型和共享，并尝试可选的云端委派。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
