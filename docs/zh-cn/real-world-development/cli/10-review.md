---
title: "第 10 课 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可重复使用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的连续工作流中使用 GitHub Copilot CLI。你：

- 准备了 Codespace、安装了 Copilot CLI、探索了项目并找到了预置的筛选议题。
- 添加了星级评分，在转发浏览器中审查结果，并手动合并了第一个拉取请求 (PR)。
- 从筛选议题开始，在 Plan 模式下确定方案，在 Autopilot 模式下构建，并在 Interactive 模式下审查。
- 使用自定义指令指导智能体，然后自定义现有的 `quality-checks` 技能并使用它运行单元测试、lint 和类型检查。
- 添加了 Playwright 模型上下文协议 (MCP) 服务器，并使用它在真实浏览器中探索筛选功能。
- 创建并选择质量保证 (QA) 自定义智能体，以评估需求、覆盖率、技能结果和浏览器证据。
- 审查了完整的筛选功能更改，并授权 Agent Merge 合并筛选 PR。
- 探索了用于管理上下文、模型、共享和可选委派给云智能体的斜杠命令。

## 你交付的内容

本研讨会包含两个 PR 里程碑，每个里程碑都从更新后的 `main` 创建独立分支：

1. **星级评分**：在游戏卡片上显示现有的 `starRating` 和明确的“尚无评分”状态。
2. **筛选功能和质量工作流**：实现筛选功能，更新指令并将其应用到功能，自定义 `quality-checks` 报告，创建 QA 配置文件，并添加相关测试。

从规划筛选功能到创建 PR，你始终使用同一个对话和分支。为简化本研讨会，我们将这些工作合并到一个 PR 中。

## 不同类型的验证

你通过多种方式检查了代码：自动测试、亲自在浏览器中检查，以及使用 MCP 让 Copilot 在浏览器中探索。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按新格式报告结果。QA 将这些结果与需求和测试覆盖率审查结合起来，然后再创建 PR。

新增测试应解决实际缺口；如果 QA 检查后认为不需要新增测试，也可能是合理的。合并前请审查代码和证据，并在更改后更新受影响的验证结果。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本研讨会中，你更新了指令、自定义了技能、创建了 QA 配置文件并配置了 MCP 服务器。可以在多个对话中重复使用这些自定义项，并根据团队需求调整。指令设定标准，技能描述可重复执行的任务，自定义智能体定义专用角色，MCP 服务器连接外部工具。请检查实际更改和工具结果，而不仅仅是智能体的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 先考虑方案，使用 **Interactive** 保持对重点更改的控制，使用 **Autopilot** 处理范围明确的任务。常规编辑可选择更快的模型，复杂任务则选择能力更强的模型。

上下文的重要性不亚于基础设施。清楚说明希望构建*什么*、*原因*和*方式*，会显著影响输出。

## 继续探索

你已完成核心工作流。还可以探索以下 CLI 功能：

- 使用 `/review` 请求代码审查智能体分析更改。
- 使用 `/rubber-duck` 讨论问题并获得另一种观点。
- 使用 `/fleet` 并行协调独立子任务。
- 使用 `/worktree` 隔离单独任务。
- 使用 `/delegate` 将任务发送给 Copilot 云智能体。

## 后续步骤

要提升对任何工具的熟练度，最好的办法就是持续使用它！可以用它处理生产代码、兴趣项目，或多年来一直想构建却尚未着手的小应用。与团队分享你的经验，也向团队成员学习。和往常一样，别忘了查看文档。

如果想继续在终端中扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若要比较其他环境，可以探索 [VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]或 [Copilot 云智能体工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
