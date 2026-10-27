---
title: "第 2 课 - 添加星级评分：快速改进"
description: "使用 Copilot CLI 修改游戏卡片，在转发浏览器中审查更改，并将其作为第一个拉取请求合并。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试对话后，现在可以为项目进行第一次更改了。先从小处着手：游戏数据中已有星级评分，但主页的游戏卡片尚未显示评分。你将要求智能体显示评分、审查更改，并将其作为第一个拉取请求合并。

本课将介绍如何：

- 在功能分支上启动专注于某项任务的 Copilot 对话。
- 要求智能体对项目进行小幅更改。
- 使用 `/diff` 审查更改。
- 运行应用，并在转发的浏览器中确认更改。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都可以有星级评分，游戏详情页已显示评分。不过，主页上的游戏卡片仅显示标题、类别、出版商和说明。作为热身练习，你将让智能体在每张卡片上显示已有评分。此更改小而独立，非常适合作为第一次会话的内容。

## 对话的组成部分

**对话**是你与 Copilot CLI 协作处理任务的环境。与 Copilot app 不同，常规 CLI 对话会使用终端当前检出的存储库和 Git 分支，而不是创建专用工作树。保存的对话可以让你之后继续之前的讨论，同时文件和分支仍是磁盘上的普通 Git 状态。

对话包含三项内容：你的提示和智能体的响应、智能体探索和编辑文件时调用的工具，以及可使用 `/diff` 检查的更改。

## 开始对话并请求更改

先启动新对话，实现此功能。

1. 返回 Codespace。
2. 如果之前打开的终端已关闭，请按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未运行，请使用以下命令启动：

   ```bash
   copilot --yolo
   ```

4. 使用斜杠命令 `/new` 并按 <kbd>Enter</kbd>，确保开始新会话。
5. 使用以下提示请求更改：

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot 会探索项目、查找用于显示游戏详情的文件并创建所需代码。你已使用 Copilot CLI 添加了新功能！

## 审查差异

合并前应审查所有 AI 生成的更改，即使是小改动也不例外。现在就在 Copilot CLI 中查看更改。

1. 输入 `/diff` 并检查每个已更改的文件。
2. 确认游戏卡片在有评分时显示数值评分，在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖了这两种状态。
4. 查看 Copilot 运行的检查结果，并要求它修复失败项。
5. 审查完成后，按 <kbd>Esc</kbd> 退出差异界面。

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 的行为具有概率性而非确定性，因此生成的代码可能有所不同。请审查其行为，不要期待某种特定实现。

## 检查更改

当然，不应只阅读代码就假定它能正常工作。让 Copilot 启动网站，然后在 Codespaces 转发的浏览器中检查更新后的用户界面 (UI)。

1. 要求 Copilot 启动应用：

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示了五分制评分。
4. 返回 Copilot，并要求它停止已启动的服务器：

   ```plaintext
   Stop the development server you started.
   ```

## 创建并合并第一个拉取请求

你已创建功能！接下来创建拉取请求 (PR)，将新代码合并到项目中。

1. 要求默认智能体提交更改：

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. 输入 `/pr create`。Copilot CLI 创建 PR 时会推送现有提交，并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Copilot CLI 显示的 URL，打开 PR。
4. 审查已更改的文件和检查结果。
5. 确认可以合并后，选择 **Merge pull request**，然后确认合并。
6. 返回 Codespace，并使用 `/exit` 退出 Copilot CLI。
7. 更新本地 `main`：

   ```bash
   git checkout main
   git pull
   ```

## 总结与后续步骤

恭喜！你已使用 GitHub Copilot CLI 发布了第一个更改！具体来说，你：

- 在功能分支上启动了专注于某项任务的 Copilot 对话。
- 引导智能体对游戏卡片进行小幅更改。
- 使用 `/diff` 审查更改。
- 运行应用，并在转发的浏览器中确认星级评分。
- 创建并合并了第一个拉取请求。

接下来，从筛选议题开始，使用 [Plan 和 Autopilot 模式][next-lesson]构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
