---
title: "第 8 课 - 创建并合并功能 PR"
description: "一并审查筛选功能、指令、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

筛选功能实现、指令更新、技能更新、质量保证 (QA) 配置文件和测试都保存在同一分支上。现在应一起审查它们并创建拉取请求。你已自行合并星级评分拉取请求 (PR)；这次将让 **Agent Merge** 管理流程。

> [!NOTE]
> 通常会将功能、指令更新、技能更新和 QA 智能体拆分到几个独立的 PR 中。为简化本研讨会，你在一个会话和分支中完成了筛选和质量工作，并将所有更改放进同一个 PR。

本课将介绍如何：

- 了解 Agent Merge 以及它如何自动管理合并生命周期。
- 检查完整的功能 PR 和验证证据。
- 审查后授权 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选功能工作流中，你使用 Copilot 规划、实现并验证了功能。现在，Tailspin Toys 希望自动处理剩余 PR 工作，同时让开发人员掌握合并授权权。

## 认识 Agent Merge

**Agent Merge** 会自动完成合并拉取请求所需的剩余工作。在 GitHub Copilot app 中启用它后，会话会读取 PR 并处理阻碍合并的事项，包括修复失败的持续集成 (CI) 检查、响应审查评论，以及在需要时执行变基。当 GitHub 允许时，它会合并 PR。此功能在后台运行，应用重启后仍会继续，并在 PR 合并后自动关闭。

到目前为止，你都是自行选择 **Merge pull request**。Agent Merge 可以代你处理这项工作，但要编辑代码和合并仍需要明确授权。授予合并权限前，请先审查其允许执行的操作和工作成果。

## 使用 Agent Merge 管理 PR

所有代码都已创建并审查完毕，现在让 Agent Merge 管理 PR 流程。

1. 在智能体选择器中选择 **Default agent**。
2. 选择 **Create PR** 旁的下拉菜单。
3. 选择 **Agent Merge**。按钮会显示为 **Agent Merge**。
4. 选择 **Agent Merge** 按钮，启动流程。仅选择下拉菜单中的选项不会启动它。

Agent Merge 流程随即启动。它将：

- 创建包含标题和说明的拉取请求。
- 如果会话从议题启动，则在说明正文中引用相关议题。
- 对目标分支执行变基或处理潜在合并冲突。
- 监控 CI 流程，确保所有检查通过。
- 监控 PR 中其他开发人员或 Copilot 代码审查的反馈，并进行更新以解决这些意见。
- 可以选择在所有操作成功后自动合并 PR。

接下来，让 Agent Merge 在所有检查通过后合并 PR。

5. 授予合并权限前，审查 PR 差异和验证证据。
6. 选择 **Agent Merge** 旁的下拉菜单。
7. 确认 **Merge pull request** 旁有勾选标记。
8. 明确要求智能体合并：

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. 跟进处理进度并解决报告的阻碍因素。确认 PR 状态为 **Merged** 后再继续；启用了合并权限或智能体仍在运行，并不代表 PR 已完成合并。

> [!IMPORTANT]
> Agent Merge 不会绕过存储库保护或缺失的权限。继续之前请解决这些阻碍因素。

## 总结与后续步骤

你已自动处理开发流程中的多个步骤，包括生成代码、测试和验证代码，以及管理拉取请求流程。本课中，你：

- 了解了 Agent Merge 以及它如何自动管理合并生命周期。
- 检查了完整的功能 PR 和验证证据。
- 仅在审查后授权 Agent Merge，并确认 PR 已合并。

接下来，[使用现有画布并创建分类画布][next-lesson]，探索与智能体一起检查、规划和可视化工作的更多方式。

## 资源

- [使用 GitHub Copilot app 管理议题和拉取请求][managing-issues-prs]
- [关于 GitHub Copilot app][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
