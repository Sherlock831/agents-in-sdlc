---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几课中，你已让 Copilot 云智能体处理了三项任务，重点是改善用户体验和添加功能。云智能体可以异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个控制中心中，可以查看所有包含未关闭拉取请求 (PR) 的智能体任务，了解已执行的操作，还可以引导正在进行的会话。

本课将介绍如何：

- 使用智能体页面监控云智能体任务。
- 引导正在进行的会话，要求添加其他功能。

## 场景

将任务分配给智能体以创建高对比度模式后，团队意识到也可以顺便添加浅色模式。由于智能体已经在更新网站样式并添加切换功能，因此加入此功能合乎情理。你将引导智能体，要求它同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务的状态。

1. 转到 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。你应能看到分配给 Copilot 的任务，包括：
    - 更新代码文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
    - 从存储库检出代码。
    - 为 Copilot 创建工作环境。
    - 设置 MCP 服务器。
    - 执行完成所分配任务所需的各种步骤。

> [!NOTE]
> 列出的具体步骤会根据 Copilot 当前的工作状态和采用的方法而变化。

4. 同时留意右侧显示的拉取请求 (PR) 窗格。你可以在此查看 PR 和已更改的文件，以便进一步监控。

## 引导云智能体

查看正在进行的任务后，要求 Copilot 在实现高对比度模式时也添加浅色模式切换开关。

1. 选择与添加高对比度模式有关的会话。确切标题可能会根据 Copilot 使用的名称和当前工作状态而有所不同。

    ![控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到会话显示已完成设置并开始工作。此时会出现类似下文的消息。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中突出显示 Steer active session while Copilot is working 对话框的截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认提示并将其加入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务！发送消息时，Copilot 可能正在执行某个步骤；完成该步骤后，它会将新请求加入工作流程。

与之前一样，这需要几分钟。你可以利用这段时间暂停一下，回顾目前学到和探索的内容。

## 总结与后续步骤

本课介绍了 Copilot 智能体页面，也就是监控和引导 GitHub Copilot 云智能体任务的中心。借助此控制中心，可以跟踪所有活动和已完成的任务、审查正在进行的工作，还可以调整正在运行的任务。

你了解了以下概念：

- 使用控制中心和智能体页面监控云智能体任务。
- 引导正在进行的会话，要求添加其他功能。

Copilot 正在完成无障碍功能，接下来将[迭代优化 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代优化 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
