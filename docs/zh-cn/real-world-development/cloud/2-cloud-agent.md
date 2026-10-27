---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债问题，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务。智能体完成工作后，会创建一个草稿 PR，供开发人员审查。这样可以将繁琐任务交给智能体，加快开发流程，让开发人员专注于更重要的工作。

本练习将介绍如何使用 Copilot 云智能体：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 了解明确界定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债需要处理。最初受雇开发网站的承包商没有妥善处理文档，你会发现项目完全缺少文档。团队希望首先为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队已准备好改善游戏发现体验。他们希望每个游戏详情页都显示相关游戏，也就是同一类别中的其他作品，让用户可以继续浏览。他们暂时不需要完善的实现，只需要能用于验收测试用户体验的版本。目前这是一个阻碍，但还有优先级更高的议题。

这两项任务都可能很快被降级，因此适合分配给 Copilot 云智能体。云智能体可以在后台异步处理任务，让开发人员继续处理其他工作，之后再回来审查 Copilot 的成果并确认一切符合预期。

## 认识 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像人类开发人员一样在后台执行任务。与人类开发人员协作的方式类似，可以通过多种方式启动任务，包括[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求来跟踪进度，设置工作环境并开始处理任务。你可以在 Copilot 会话进行中或完成后查看详情。解决方案准备好供审查时，Copilot 会在拉取请求中提及你。

## 明确界定指令范围的重要性

GitHub Copilot 没有魔法，不能替你省去思考。即使看似简单的操作，深入分析后也会发现其复杂性，因此要[谨慎确定 Copilot 云智能体的任务范围][cloud-agent-best-practices]。将它视为 AI 结对编程助手：分阶段工作、学习、试验并根据情况调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令及其指导 Copilot 的方式。如果你已完成自定义指令练习，就会看到 `.github/copilot-instructions.md` 的作用；如果尚未学习，现在可以快速查看。将工作分配给 Copilot 云智能体之前，请只读查看本存储库中的指令文件，以便稍后观察它们的影响。

在存储库的 GitHub Web 界面中打开以下文件；如果已启动 codespace，也可以在那里打开：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释方面的要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将指令限定在 `**/*.test.ts` 文件。

在下一节中，将 `Code lacks documentation` 议题分配给云智能体时，留意它创建的拉取请求是否包含 TSDoc 文档注释、TypeScript 约定和注释头。这些要求来自上述指令文件。稍后的练习中审查 PR 时还会再次提到这些要求。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要准备特定环境并运行一些设置脚本，以确保环境正常。将任务分配给 Copilot 时也是如此，因为它会像软件工程师一样执行任务。

云智能体使用 [GitHub Actions][github-actions] 环境。你可以通过在 `.github/workflows/copilot-setup-steps.yml` 文件中配置[专用设置工作流][setup-workflow]来自定义环境，让它在开始工作前运行，并获得所需的开发工具和依赖项。为了让本实验顺利进行，该工作流已预先配置好，确保 Copilot 可以使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及为单体 Astro 应用迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可在存储库的 "Actions" 选项卡中测试这些设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限设为这些步骤所需的最低级别。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，Copilot 会在步骤完成后自动克隆。
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

      # 迁移本地 SQLite 数据库并填充初始数据，供构建和测试使用。
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流文件类似，但有几个关键点：

- 它包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- 留意 `workflow_dispatch` 触发器。借助它，可以在存储库的 **Actions** 选项卡中手动运行工作流。这有助于测试工作流是否能成功运行，而不必等待 Copilot 启动它。

## 添加文档

每个人都知道文档的重要性，但大多数项目的信息要么已过时，要么完全缺失。这类技术债常常得不到处理，进而降低工作效率，也增加维护代码库或引入新团队成员的难度。Copilot 擅长创建文档，因此这正是适合分配给云智能体的议题。它会在后台生成所需文档，稍后的练习中将审查其成果。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

    ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求 (PR)，其标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中没有出现新的 PR，请等待一两分钟，然后刷新浏览器窗口。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 的待办事项列表可能需要几分钟才会显示在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定处理问题的最佳方法。

13. 查看 Copilot 即将完成的任务列表。
14. 向下滚动拉取请求时间线，应能看到 Copilot 已开始处理此议题的更新。
15. 选择 **View session** 按钮。

    ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的指示器。

16. 浏览实时会话，查看 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并确定合适的计划，以及编写代码。

这可能需要几分钟。Copilot 云智能体的主要目标之一是异步执行任务，让我们可以专注于其他工作。接下来会利用这一点：先将另一项任务分配给 Copilot 云智能体，再转而编写代码为应用添加功能。

## 在游戏详情页中添加相关游戏部分

如前所述，GitHub Copilot 云智能体的一大优势是可以分工处理任务，让你专注于其他工作。为设计团队添加相关游戏部分可能不会花很长时间，但也可以利用这段时间处理其他任务。现在就将此任务分配给 Copilot 云智能体。

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
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

    ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后，你会在议题的第一条评论中看到一组 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。Copilot 云智能体的工作方式与软件工程师类似，因此无需一直监控它；等任务完成后再审查即可。现在可以转而创建和使用自定义智能体。

## 总结与后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以使用云智能体异步处理分配给 Copilot 的议题，例如解决技术债、创建新功能，或协助将代码从一种框架迁移到另一种框架。

你了解了以下概念：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 明确界定议题范围的重要性。
- 将议题分配给 Copilot。

云智能体正在后台处理任务，现在可以转而创建和使用自定义智能体。[Copilot 云智能体也可以使用 MCP 服务器][cloud-agent-mcp]，并且可以使用前面模块中介绍的自定义指令。

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
