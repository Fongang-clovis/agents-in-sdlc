---
title: "第 12 课 - 使用 GitHub Pages 部署网站"
description: "使用 GitHub Copilot app 配置 Pages 工作流、审查拉取请求，并确认 Space Quiz 已上线。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

使用 GitHub Pages 部署 Space Quiz，为它提供公开访问入口。Copilot 可以准备工作流并指导部署，但仍需你审查更改并确认网站已经上线。

本课将：

- 让 Copilot 配置 GitHub Pages 部署；
- 在合并前审查工作流和拉取请求；
- 跟进部署流程并确认网站已发布。

## 场景

Space Quiz 已在 GitHub 存储库中，但用户还需要一种简单方式来试用它。使用 GitHub Pages 发布单文件网站，无需添加服务器或运行时依赖项，即可通过公开 URL 访问。

## 让 Copilot 部署网站

在 Copilot app 的 `space-quiz` 会话中，发送以下提示词：

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![三阶段示意图：已合并的 space-quiz 拉取请求；成功运行的 GitHub Actions Deploy website 工作流，其中包括 Build website、Upload artifact 和 Deploy to Pages 步骤；以及已上线的 Space Quiz 网站，地址为 yourname.github.io/space-quiz/。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> 此提示词要求智能体创建并合并拉取请求，以及更改存储库 Pages 设置。继续前，请审查差异、部署权限和 GitHub 审批提示。

## 审查工作流并验证部署

1. 确认只有在存储库中尚无 README 时才添加该文件。
2. 审查拉取请求更改，包括工作流中的构建和部署步骤。
3. 只有必需的拉取请求检查全部通过后才合并。
4. 跟踪 GitHub Actions 部署运行，直到成功。
5. 打开报告中的 URL，确认 Space Quiz 已加载。

如果智能体无法启用 Pages 或合并拉取请求，请按照它提供的具体说明操作。只有部署成功且网站可以访问后，才能将任务视为完成。

## 总结和下一步

Space Quiz 已作为实时网站发布，并且你亲自验证了部署，而不是只依赖智能体的报告。继续学习[第 13 课：回顾与后续步骤][next-lesson]。

[next-lesson]: ../13-review/
