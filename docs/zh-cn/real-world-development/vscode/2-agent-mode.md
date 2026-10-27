---
title: "练习 2 - 使用 Copilot 智能体模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。作为开发人员，通常需要查找所有相关文件、进行更改、运行测试、调试、找出遗漏的文件并再次更新……事情往往会不断重复。

Copilot 智能体模式正是为此而设计的。

Copilot 智能体模式可在 IDE 中更自主地工作。它的工作方式类似开发人员：先探索现有项目结构，执行所需更新，运行测试等任务，并自动修复发现的问题。接下来将了解如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体的名称相似，但它们适用于不同的工作方式。智能体模式在 IDE 中执行任务，便于快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将介绍如何：

- 使用 Copilot 智能体模式探索项目、识别相关文件并协调更改。
- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 在将更改合并到代码库之前，审查 Copilot 智能体模式生成的更改和测试。

## 场景

随着游戏列表不断增加，你希望允许用户按类别和出版商筛选。上一课已添加出版商辅助函数，现在将使用 Copilot 智能体模式完成数据层、UI 和测试方面的其他工作。

## 运行 Tailspin Toys 网站

进行任何更改前，先探索 Tailspin Toys 网站，了解它目前的功能。

此网站是一个以开发者为主题的桌游众筹平台，用户可以浏览游戏列表和详情。网站是一个单体 Astro 应用，会在构建时将页面呈现为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可重复使用的数据访问辅助函数放在 `src/lib/` 中；交互功能使用小型、限定范围的 Astro `<script>` 和标准 DOM API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单体 Astro 开发服务器。可以按照以下步骤在 GitHub Codespace 中运行：

1. 返回 codespace。上一课结束后，你应该已切换回 `main`。
2. 在 Codespace 中按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），打开新终端窗口。
3. 为筛选功能创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器准备就绪后，会显示类似以下内容的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否要为 `http://localhost:4321` 打开浏览器窗口，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中，从终端选择 localhost URL 时，系统会自动将你重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 codespace（此时正在运行 Web 服务器）的私有隧道。

### 浏览网站

网站运行后，可以探索其功能。主要功能包括：

- **主页**：显示桌游列表，包括标题、图像和说明。
- **游戏详情页**：选择游戏后，会打开详情页，显示标题、说明、出版商和类别等信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板，并选择 **New Chat** 开始干净的对话。不同练习所用的模式和模型可能不同；每个步骤会在需要时说明。

网站初始版本可以正常运行，但还需要添加更多功能。先查看待办事项列表。通过模板设置项目时，系统已自动创建一组议题；让 GitHub Copilot 帮你显示这些议题。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。**Agent** 智能体可自主规划和实现跨文件更改、运行终端命令并调用工具。

   ![Chat 视图中的智能体选择器截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写本研讨会时使用的是 Claude Sonnet 4.5，因此课程选择该模型，希望获得相对一致的建议，确保良好体验。不过，由于 LLM 的行为具有概率性，你可能会收到与课程不同的建议。这完全正常，也在预期之中。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问议题待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 以运行列出所有议题的命令。
5. 查看生成的议题列表。

留意 Copilot 还根据对用户的帮助程度为待办事项确定了优先级。

## 查看指令文件

让智能体生成代码之前，先查看将用于向 Copilot 提供上下文的指令文件。本练习将使用[用户界面 (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) 文件，其中包含如何为网站添加功能的上下文。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中关于如何添加功能的总体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向各文件类型对应指令文件的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指令文件可以引用其他指令文件，也可以引用项目中的文件。路径相对于指令文件所在位置，因此可以重复使用指导内容、将大型指令集拆分成更易管理的小文件，并提供示例和模板。

## 实现筛选功能

完成筛选至少需要对应用进行三项不同的更新：

- 在数据层 (`src/lib/`) 中添加或完善其余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选 UI。

此外，合并前还需要运行并通过测试。Copilot 智能体模式可以完成这些任务，接下来开始实现功能。

1. 可以继续使用当前 Copilot 对话，也可以选择 **New Chat** 创建新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍选择了 **Agent**。

   ![Chat 视图中的智能体选择器截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认仍为模型选择 **Claude Sonnet 4.5**。
5. 使用待办事项中的相关议题提示 Copilot 实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并查找相关文件。它应该会找到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 正在探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。在此过程中，编辑器中可能会显示 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会提供 **Keep** 或 **Undo** 操作，因此无需在工作过程中选择它们。

7. Copilot 提示运行测试时，选择 **Continue**。

   ![Copilot Chat 面板中询问用户是否允许运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会出现暂停，也可能会看到测试失败。这没有关系！Copilot 会在代码生成和测试之间反复迭代，直到完成任务且不再检测到错误。

   ![Copilot 智能体模式完整聊天会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查看是否存在潜在问题。

> [!CAUTION]
> 请记住，始终需要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，探索新功能。
11. 确认功能正常工作并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结与后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加新功能。你了解了：

- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能。
- Copilot 智能体模式如何探索项目、识别相关文件并协调更改。
- 如何在将代码合并到代码库之前审查 Copilot 智能体模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]并为其创建拉取请求。

### 附加探索练习 - 实现分页

随着游戏列表增长，网站将需要分页功能。运用本练习中学到的技能，提示 Copilot 更新网站，实现分页。编写代码时可考虑：

- 遵循现有最佳实践，包括使用已有指令文件。
- 考虑分页方式：允许用户选择每页大小，还是使用固定值。
- 编写提示时，提供必要指导，确保 Copilot 按期望实现功能。
- 你可能需要与 GitHub Copilot 迭代，要求它做出更改并提供上下文。这是使用 Copilot 时的正常工作方式！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：用途及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
