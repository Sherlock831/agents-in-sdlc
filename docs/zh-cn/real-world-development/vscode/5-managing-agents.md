---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

使用智能体模式时，GitHub Copilot 会自主探索代码库、提出更改建议、编辑文件并运行命令。由于这些工作在本机实时进行，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。你可以直接在聊天中审查每个差异、接受或拒绝单项更改，并通过后续提示引导对话、调整或扩展工作，全程无需离开编辑器。

本练习将介绍如何：

- 实时监控无障碍智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话进行期间引导智能体，在高对比度模式旁添加浅色模式切换开关。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义智能体正在实现高对比度模式。工作进行期间，设计团队提出了额外需求：他们也希望为觉得默认主题太暗的用户添加浅色模式切换开关。无需从头开始新对话，你可以引导当前会话扩展工作范围。

## 监控 Copilot 的进度

Copilot Chat 视图会显示智能体执行的每个步骤，包括探索的文件、运行的命令和建议的编辑。每个文件编辑都会以内联差异的形式显示，并提供 **Keep** / **Undo** 控件。

1. 返回练习 4 中使用的 Copilot Chat 视图。
2. 智能体工作时，滚动聊天内容查看每次工具调用。你会看到文件读取、文件编辑以及智能体请求运行的终端命令。
3. 智能体建议编辑文件时，差异会以内联方式显示：
    - 选择 **Keep** 接受更改。
    - 选择 **Undo** 撤销更改，让智能体重新尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看智能体在本次会话中修改的完整文件列表。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你信任的命令。

5. 等待智能体完成高对比度模式的实现。完成后应会看到总结更改内容的最终消息。

## 引导智能体添加浅色模式

现在高对比度模式已经就绪，可以在同一对话中扩展工作，添加浅色模式切换开关。继续使用当前聊天会话，智能体会保留之前工作的所有上下文，包括修改的组件、使用的样式方案和本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 智能体会继续处理任务，探索此前已经进行的更改，并为浅色模式提出补充内容。
3. 与高对比度模式一样，逐项审查新编辑。使用 **Keep** 或 **Undo** 处理每个差异。
4. 如果智能体采取了不符合预期的方向（例如尝试以不喜欢的方式重构切换 UI），可以选择聊天输入框旁的 **Stop** 按钮停止任务，然后发送说明进一步澄清。

> [!TIP]
> 与其从头开始，不如引导正在运行的会话。可以使用后续提示要求调整（“简化 CSS，使用 CSS 变量而不是重复选择器”）、纠正方向（“不要添加第三个切换开关，将浅色模式集成到现有设置面板”），或扩展范围（“也为两个切换开关添加键盘快捷键”）。

## 审查工作集

提交工作前，快速检查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审查更改文件的完整列表。应能看到 Astro 组件、样式和相关测试的更新。
3. 打开几个已更改的文件并查看差异。确认其中体现了自定义智能体提出的无障碍模式，包括 ARIA 属性、键盘导航、语义化 HTML 和本地存储持久化。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课将发布这些更改。

## 总结与后续步骤

你使用 Copilot Chat 智能体模式监控并引导正在进行的会话，逐项审查文件编辑、接受或拒绝更改，并在保留上下文的情况下发送后续提示扩展工作。

本课介绍了以下概念：

- 实时监控智能体工作。
- 接受或拒绝单项文件编辑。
- 在会话进行期间通过后续提示引导智能体。

在[下一课][next-lesson]中，你将通过拉取请求发布无障碍相关工作，并审查本实验中本地自定义智能体生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代优化 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
