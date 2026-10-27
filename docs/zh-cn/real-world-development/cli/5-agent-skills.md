---
title: "第 5 课 - 自定义和使用 quality-checks 技能"
description: "查看现有的 quality-checks 技能、自定义结果报告格式，并用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码不只是编写代码。我们已手动验证代码能够运行，也使用指令文件确保代码符合标准。那么测试、lint 以及持续集成 (CI) 中的其他步骤又该如何处理？

对于这类任务，**智能体技能**最为合适！技能可以帮助 Copilot 了解如何正确执行这些操作。

本课将介绍如何：

- 查看现有的 `quality-checks` 技能。
- 自定义其结果格式。
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告，让结果更容易阅读。

## 指令、脚本和资源

智能体技能会打包可重复使用的任务指令、可执行脚本和支持资源，供智能体按需加载。技能的核心是一个以技能名称命名的文件夹，其中包含 Markdown 文件 `SKILL.md`。Markdown 文件中包含用于定义技能名称和说明的 frontmatter、技能概述，以及关于何时调用技能的指导。文件夹中还可以包含供技能调用时使用的脚本和其他资源子文件夹。

> [!NOTE]
> 技能不一定需要额外的文件和文件夹。Tailspin Toys 的 `quality-checks` 技能仅包含 `SKILL.md`，因为它使用项目现有命令。

技能可以放在项目的 `.github/skills` 文件夹中，作为存储库资源供团队共享和重复使用；也可以放在用户技能文件夹 `~/.copilot/skills` 中。

## 查看技能

现在查看 Tailspin Toys 团队用于运行单元测试、lint 和类型检查的 `quality-checks` 技能。

1. 返回 Codespace。在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读顶部的 `name` 和 `description`。说明有助于 Copilot 了解何时调用技能。
3. 阅读指令，留意它们如何引导 Copilot 执行测试和 lint 流程。
4. 留意技能中尚无 **Results output formatting** 部分。

## 更改前运行技能

可以直接通过 Copilot CLI 调用技能，也可以使用自然语言请求。现在让 Copilot 运行该技能的三项检查。

1. 返回筛选功能对话，并切换到 Interactive 模式。
2. 使用以下提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最终报告。

## 自定义报告

我们希望报告能说明运行了哪些检查、是否成功，以及工具实际报告了什么。现在更新技能，生成这种报告。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

更改完成后，看看效果如何！Copilot CLI 无需重启对话即可重新加载编辑后的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 使用之前的相同提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最终报告，并与第一份报告进行比较。

## 总结与后续步骤

你已自定义并使用现有智能体技能。本课中，你：

- 查看了用于单元测试、lint 和类型检查的 `quality-checks` 技能。
- 自定义了技能的结果格式。
- 重新加载并运行了技能。

这些更改会与筛选功能一起包含在功能 PR 中。接下来，通过 [Playwright MCP 服务器][next-lesson]让 Copilot 直接与网站交互。

## 更多技能示例

以下社区示例仅供参考，不是额外任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表及其打包的导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
