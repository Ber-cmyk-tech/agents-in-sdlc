---
title: "第 3 课：代理模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案、使用 Autopilot 根据 issue 实现筛选，并在 Interactive 模式中审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们先为项目添加了小功能，但更大的更改需要更完善的流程。GitHub Copilot CLI 可以融入组织现有工作流，帮助团队以正确方式构建正确功能。本课是几节课中的第一节，你将遵循典型的代理式开发流程：从 issue 开始实现新功能，确认代码有效、功能符合预期，最后成功合并到项目。

> [!NOTE]
> 后续功能工作将沿用同一对话和分支。通常会为不同类型的文件创建独立分支或 PR，但本工作坊会简化流程，帮助你专注于核心概念。

本课将学习如何：

- 从 GitHub issue 启动新的 Copilot 对话。
- 在 Plan 模式中定义要求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续实现此功能时，你还将更新仓库指令、自定义现有 `quality-checks` 技能、添加 MCP 验证、创建 QA 代理并打开功能 PR。

## 场景

Tailspin Toys 的目录不断扩充，访客需要按类别和发行商缩小游戏范围。待办 issue 描述了功能，但组合类别等细节需要在编码前确定。你将使用 Plan 模式确定这些决策，然后批准 Autopilot 在限定范围内实现功能。

## 背景

将 AI 编码代理引入开发流程不会改变基本原则，反而让这些原则更加重要！大多数开发人员的工作流程大致如下：

1. 从已创建的 issue 开始，了解需要完成的工作。
2. 制定实现计划。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 每个团队和组织的具体流程都不相同，但大多数流程都与上述步骤类似。

遵循标准流程可以确保 AI 生成的代码满足需求，并经过与手动编写的代码相同的验证。

## 对话模式

**对话模式**控制代理的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：你与代理协作。代理提出更改建议，并等待你的输入后再继续。
- **Plan**：代理先制定计划，但不能编辑项目文件。
- **Autopilot**：代理自主编写代码、运行测试并持续迭代，直至任务完成。

先使用 Plan 模式，审查计划后再用 Autopilot 实现。

## 从 issue 开始

开始筛选工作前，返回 Codespace 并确认仓库和终端已准备就绪：

1. 返回 Codespace。如果它已停止，请先重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）。
4. 更新 `main` 并为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有 issue。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的 issue。
9. 按 <kbd>C</kbd> 将 issue 添加到提示中并返回 **Session** 选项卡。

现在提示中应以 `#7`（或类似编号）开头。`#` 可将 GitHub issue 或拉取请求（PR）添加到上下文中。

## 规划筛选功能

规划可以先确定实现功能或执行任务的方法，再交给 Copilot。复杂任务通常应花些时间规划。现在切换到 Plan 模式，让 Copilot 创建计划：

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的 issue 引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会先探索项目，再确定最佳方案并制定计划。

3. Copilot 可能会询问筛选行为的细节。根据偏好作答，没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 滚动查看计划。
6. 如果计划中的任何部分不符合你的决定，要求 Copilot 修改。

## 批准 Autopilot

计划编写并审查完成后，让 Copilot 开始实现：

1. 选择 **Accept plan and build on autopilot (recommended)**，或你所用版本中类似的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 如何根据既定计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 审查并验证实现

与其他代码一样，生成的代码也必须在合并前审查。审查代码并运行网站，确认一切正常：

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选实现和测试。
3. 将结果与 issue 和规划阶段的决定进行比较。
4. 审查完成后按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果，并要求 Copilot 修复任何失败项。

## 试用新功能

代码看起来不错，但是否真的能运行？像之前一样启动应用，通过 Codespaces 转发端口访问网站：

1. 要求 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. 当 Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及计划中确定的组合筛选。
4. 确认重置和无结果状态符合 issue 和你的决定。
5. 返回 Codespace，要求 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你使用不同的对话模式来构建并审查功能。本课中，你：

- 从 GitHub issue 启动了新的 Copilot 对话。
- 在 Plan 模式中定义要求。
- 使用 Autopilot 模式实现新功能。
- 审查了代码。
- 在转发的浏览器中手动验证功能。

接下来，[使用自定义指令][next-lesson]深入了解代码生成，确保 Copilot 遵循文档化的实践。

## 资源

- [Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
