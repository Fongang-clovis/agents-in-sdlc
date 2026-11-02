---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都会面对技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务。任务完成后，它会创建一个草稿 PR，供开发人员审查。这样可以将繁琐任务交给智能体，加快开发进程，让开发人员专注于更重要的工作。

本练习将探索如何：

- 自定义代码生成环境；
- 确保操作安全；
- 编写范围明确的议题；
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初创建网站的承包商没有留下文档，因此团队希望先为应用程序中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现体验：在每款游戏的详情页上展示同一类别中的相关游戏，让访客继续浏览。他们暂时不需要成熟的实现，只需要可以进行用户验收测试的版本。目前这项工作受阻，但其他议题优先级更高。

这类任务很容易被推迟，因此适合分配给 Copilot 云智能体。它可以异步处理任务，让开发人员先处理其他工作，之后再回来审查结果。

## 认识 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发人员一样在后台执行任务。可以通过多种方式启动任务，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求来跟踪进度、设置环境并开始处理任务。你可以在会话进行中或完成后查看它的工作。准备好审查建议的解决方案时，它会在 PR 中标记你。

## 编写范围明确的指令

GitHub Copilot 并非魔法工具，不能替代思考。即使看似简单的操作，深入分析后也会发现复杂性，因此请注意如何为 Copilot 云智能体设定任务范围，遵循[云智能体最佳实践][cloud-agent-best-practices]。将它视为 AI 结对程序员：分阶段工作、学习、试验并不断调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令及其作用。如果已经完成自定义指令练习，就见过 `.github/copilot-instructions.md` 的效果；如果还没有，可以现在快速查看。在将工作分配给 Copilot 云智能体之前，先以只读方式查看存储库中已有的指令文件，稍后便能观察其影响。

在 GitHub Web 界面或已启动的 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 章节，尤其注意 TypeScript 规范和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将指令限定在 `**/*.test.ts` 文件。

下一部分会将 `Code lacks documentation` 议题分配给云智能体。观察生成的 PR 是否遵循这些指令中的 TSDoc 注释、TypeScript 规范和文件注释头。稍后的练习还会再次审查这些内容。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，确保环境就绪。将任务分配给以类似软件工程师方式工作的 Copilot 时也是如此。

云智能体使用 [GitHub Actions][github-actions] 执行任务。可以创建[专用设置工作流][setup-workflow]，在 `.github/workflows/copilot-setup-steps.yml` 中配置，并在智能体开始工作前运行，以便安装所需的开发工具和依赖项。为便于本实验学习，这个文件已预先配置好，确保 Copilot 可以使用 Node.js、项目依赖项、端到端测试所需的浏览器，以及已迁移并填充数据的 SQLite 数据库（本项目是单个 Astro 应用程序）：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的“Actions”选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为这些步骤配置所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果未在设置步骤中克隆，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在基于 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试使用其中的数据。
      - name: Set up the database
        run: npm run db:setup
```

该文件与其他 GitHub 工作流类似，但有几点值得注意：

- 它包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理 PR 前，GitHub Actions 会运行此作业。
- `workflow_dispatch` 触发器可让你从存储库的 **Actions** 选项卡手动运行工作流。这有助于测试设置流程，无需等待 Copilot 启动。

## 添加文档

大家都了解文档的重要性，但大多数项目的信息要么已过时，要么完全缺失。这类技术债务经常无人处理，会拖慢开发、增加维护难度，也让新成员更难加入。Copilot 擅长生成文档，因此这是分配给云智能体的理想任务。它会在后台生成所需文档，稍后可回来审查。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的 PR，标题大致为 `[WIP]: Code lacks documentation`。如果列表中还没有新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才会在 PR 中显示待办事项列表。它需要创建环境（运行前面介绍的工作流）、分析项目并确定处理问题的方案。

13. 查看待办事项以及 Copilot 准备完成的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态。

16. 浏览实时会话，观察 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并选择合适方案，以及编写代码。

这可能需要几分钟。云智能体的主要优势之一是异步执行任务，让你可以处理其他工作。接下来利用这一点，再分配一项任务给云智能体，同时开始为应用添加功能。

## 在游戏详情页添加相关游戏

Copilot 云智能体的一大优势是可以拆分工作：你专注于一组任务，它同时处理另一组。即使为设计团队添加相关游戏不需要太长时间，也可以把时间留给其他工作。现在将此任务分配给云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，议题的第一条评论中会出现一组 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求！它的工作方式类似软件工程师，不需要你一直监控，完成后再进行审查即可。接下来了解如何创建和使用自定义智能体。

## 总结和下一步

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体，让它异步处理技术债务、创建新功能，或协助迁移代码框架。

本课介绍了以下概念：

- 自定义代码生成环境；
- 确保操作安全；
- 编写范围明确的议题；
- 将议题分配给 Copilot。

云智能体正在后台工作，接下来可以了解如何创建和使用自定义智能体。[云智能体也可以使用 MCP 服务器][cloud-agent-mcp]，并可使用前面练习中介绍的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
