---
title: "练习 5：监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义代理][previous-lesson] |
|:--|

启用 GitHub Copilot 代理模式后，它会自主探索代码库、提出更改建议、编辑文件并运行命令。由于这些工作在本机实时执行，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。你可以在编辑器中查看每个差异、接受或拒绝单项更改，并通过后续提示引导会话，完善或扩展工作。

本练习将学习如何：

- 实时监控无障碍代理的工作。
- 审查并接受建议的文件编辑。
- 在会话进行时引导代理添加浅色模式切换。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义代理正在实现高对比度模式。工作期间，设计团队提出了新需求：他们也希望为觉得默认主题太暗的用户添加浅色模式。你将引导现有会话扩展工作，而不是从头开始新会话。

## 监控 Copilot 的进度

Copilot Chat 视图会显示代理执行的每个步骤，包括探索的文件、运行的命令和提出的编辑。每项文件编辑都以内联差异的形式显示，并提供 **Keep** 和 **Undo** 控件。

1. 返回练习 4 中使用的 Copilot Chat 视图。
2. 代理工作时，滚动聊天内容查看每次工具调用。你会看到文件读取、文件编辑和请求运行的终端命令。
3. 代理提出文件编辑时，差异会以内联方式显示：
    - 选择 **Keep** 接受更改。
    - 选择 **Undo** 撤销更改，让代理再次尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看本会话中代理修改的全部文件。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），代理会暂停并显示 **Allow** 提示。只批准你认可的命令。

5. 等待代理完成高对比度模式实现。完成时会显示更改摘要。

## 引导代理添加浅色模式

高对比度模式实现后，在同一会话中继续要求添加浅色模式。由于会话上下文仍然保留，代理能沿用之前的组件、样式方案和本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 代理会检查之前的更改，并提出添加浅色模式的修改。
3. 像审查高对比度模式一样审查每项新编辑，并使用 **Keep** 或 **Undo**。
4. 如果代理的实现方向不符合预期（例如它想重构切换 UI），可以选择聊天输入框旁的 **Stop** 按钮停止任务，然后发送澄清后的后续提示。

> [!TIP]
> 与重新开始相比，引导正在执行的会话更快。可以用后续提示要求调整（“简化 CSS，使用 CSS 变量而不是重复选择器”）、纠正方向（“不要添加第三个开关，将浅色模式整合到现有设置面板”）或扩展范围（“同时为两个开关添加键盘快捷键”）。

## 审查工作集

提交前快速检查代理修改的全部内容：

1. 在 VS Code 中打开 **Source Control** 视图。
2. 查看已更改文件列表。应该能看到 Astro 组件、样式和相关测试的更新。
3. 打开一些已更改文件查看差异。确认其中体现了自定义代理要求的无障碍模式，包括 ARIA 属性、键盘导航、语义化 HTML 和本地存储。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结和后续步骤

你使用 Copilot Chat 代理模式监控并引导正在进行的会话：查看每项文件编辑、接受或拒绝更改，并通过后续提示扩展工作，同时保留原有上下文。

本课介绍了以下内容：

- 实时监控代理工作。
- 接受或拒绝单项文件编辑。
- 通过后续提示在会话进行时引导代理。

下一课将[发布无障碍功能的拉取请求][next-lesson]，并审查本实验中本地自定义代理生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用代理模式][agent-mode]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
