---
title: "练习 4：监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几次练习中，你让 Copilot cloud agent 承担了三项改进用户体验和添加功能的任务。Cloud agent 虽然可以异步自主工作，但监控任务仍然十分重要。

你可以使用多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的[代理页面][agents-page]。在这个控制中心可以查看所有有未关闭拉取请求（PR）的代理任务、查看它们执行的操作，甚至引导正在进行的会话。

本课将学习如何：

- 使用代理页面监控 cloud agent 任务。
- 引导正在进行的会话，要求添加其他功能。

## 场景

将高对比度模式分配给代理后，团队意识到也可以顺便添加浅色模式。由于代理正在更新网站样式并添加切换功能，加入浅色模式很合适。你将引导代理，让它同时实现浅色模式和高对比度模式。

## 查看 Copilot cloud agent 任务

查看当前分配给 Copilot cloud agent 的任务状态：

1. 打开 [Copilot 代理页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。你应能看到分配给 Copilot 的任务，包括：
    - 更新代码文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择一项正在运行的任务，查看 Copilot 已执行的操作，例如：
    - 从仓库检出代码。
    - 创建 Copilot 工作环境。
    - 设置 MCP 服务器。
    - 执行完成任务所需的各项操作。

> [!NOTE]
> 具体步骤会根据 Copilot 的工作状态和执行方式而变化。

4. 同时留意右侧的拉取请求（PR）面板。你可以在此查看 PR 和更改的文件，以便进一步监控。

## 引导 cloud agent

查看当前活动任务后，要求 Copilot 在实现高对比度模式的同时也加入浅色模式：

1. 选择与添加高对比度模式有关的会话。具体标题取决于 Copilot 使用的名称和当前工作状态。

    ![代理控制台中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 等待几分钟，直到会话显示已完成设置并开始工作。此时会出现类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![代理页面中的 cloud agent 任务，突出显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认提示并将它纳入工作流程。

## 等待 Copilot 完成工作

和之前一样，Copilot 会继续处理更新后的任务！收到消息时，它会先完成当前步骤，然后将新要求纳入工作流程。

这也需要几分钟。你可以利用这段时间回顾到目前为止学到的内容。

## 总结和后续步骤

本课介绍了 Copilot 代理页面，这是监控和引导 GitHub Copilot cloud agent 任务的中心位置。你可以跟踪所有正在进行和已完成的任务、审查工作成果，并重定向正在进行的任务以调整范围或补充指导。

你学习了以下内容：

- 使用代理控制台和代理页面监控 cloud agent 任务。
- 重定向正在进行的会话以请求添加其他功能。

Copilot 正在完成无障碍功能。接下来，[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 代理页面][agents-page]
- [自定义代理简介][custom-agents]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
