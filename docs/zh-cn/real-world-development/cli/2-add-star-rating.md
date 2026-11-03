---
title: "第 2 课：添加星级评分，快速完成一项改进"
description: "使用 Copilot CLI 修改游戏卡片，在转发浏览器中审查结果，并将其作为你的第一个拉取请求合并。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试过对话后，现在来为项目做第一个更改。先从小处着手：游戏数据中已有星级评分，但主页的游戏卡片尚未显示评分。你将让代理显示评分、审查更改并将它作为第一个拉取请求合并。

本课将学习如何：

- 在功能分支上启动专注于单一任务的 Copilot 对话。
- 让代理对项目进行小幅更改。
- 使用 `/diff` 审查更改。
- 启动应用，在转发的浏览器中确认更改。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都可以有星级评分，游戏详情页已显示评分。但主页的游戏卡片只显示标题、类别、发行商和说明。作为热身练习，你将让代理在每张卡片上显示已有评分。此更改范围小且独立，很适合作为第一次会话的任务。

## 对话的组成

**对话**是你与 Copilot CLI 协作完成任务的空间。与 Copilot app 不同，普通 CLI 对话使用终端当前检出的仓库和 Git 分支，而不是创建专用工作树。保存的对话可以让你稍后返回同一讨论，而文件和分支仍是磁盘上的普通 Git 状态。

对话中有三项内容：你发送的提示和代理回复、代理探索及编辑文件时的工具活动，以及通过 `/diff` 检查的更改。

## 开始对话并请求更改

开始新的对话来实现功能：

1. 返回 Codespace。
2. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未运行，执行以下命令：

   ```bash
   copilot --yolo
   ```

4. 使用斜杠命令 `/new` 并按 <kbd>Enter</kbd>，确保开始新会话。
5. 使用以下提示请求更改：

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot 会探索项目、查找显示游戏详情的文件并编写所需代码。现在你已使用 Copilot CLI 添加了新功能！

## 审查差异

所有 AI 生成的更改都应在合并前审查，即使更改很小也不例外。现在在 Copilot CLI 中查看更改：

1. 输入 `/diff` 并检查每个更改的文件。
2. 确认游戏卡片在有评分时显示数字评分，在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖这两种状态。
4. 查看 Copilot 运行的检查结果，并要求它修复任何失败项。
5. 审查完成后，按 <kbd>Esc</kbd> 退出差异界面。

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 具有概率性而非确定性，因此最终代码可能有所不同。请审查实际行为，而不是要求实现与某个方案完全一致。

## 检查更改

不能只阅读代码就假定它可用。让 Copilot 启动网站，以便在 Codespaces 转发的浏览器中查看更新后的 UI：

1. 要求 Copilot 启动应用：

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. 当 Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示五星制评分。
4. 返回 Copilot，要求它停止启动的服务器：

   ```plaintext
   Stop the development server you started.
   ```

## 创建并合并第一个拉取请求

你已创建功能，现在通过拉取请求（PR）将代码合并到项目中。

1. 要求默认代理提交更改：

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. 输入 `/pr create`。Copilot CLI 会在创建 PR 时推送现有提交，并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Copilot CLI 显示的 URL 打开 PR。
4. 审查更改的文件和检查结果。
5. 准备就绪后，选择 **Merge pull request** 并确认合并。
6. 返回 Codespace，输入 `/exit` 退出 Copilot CLI。
7. 更新本地 `main`：

   ```bash
   git checkout main
   git pull
   ```

## 总结和后续步骤

恭喜！你使用 GitHub Copilot CLI 发布了第一个更改！具体来说，你：

- 在功能分支上开始了专注于单一任务的 Copilot 对话。
- 让代理小幅修改游戏卡片。
- 使用 `/diff` 审查更改。
- 在转发的浏览器中运行应用并确认星级评分。
- 创建并合并了第一个拉取请求。

接下来，从筛选 issue 开始，使用 Plan 和 Autopilot 模式[构建更大的功能][next-lesson]。

## 资源

- [GitHub Copilot CLI 简介][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
