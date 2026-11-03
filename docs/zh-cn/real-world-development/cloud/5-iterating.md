---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理代理][previous-lesson] |
|:--|

## 审查工作成果

在整个实验中，你与 GitHub Copilot 一起完成了多项改进用户体验和添加功能的任务：为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现高对比度和浅色模式切换等无障碍功能。现在查看代码更改；必要时向 Copilot 提供反馈，进一步完善成果。之后再为团队启用代码审查。

### 场景

即使加入生成式 AI，软件设计和 DevOps 的基本原则也不会改变。我们仍应审查生成的代码，并遵循常规 DevOps 流程。接下来，在为其他团队成员启用审查前，先查看 GitHub Copilot 为文档、相关游戏功能和无障碍功能提出的更改。

## 安全与 GitHub Copilot cloud agent

由于 Copilot cloud agent 会异步且无人监管地工作，系统设定了安全限制：

- Copilot 对仓库只有读取权限，只能写入它执行任务的分支。
- Cloud agent 在 GitHub Actions 中运行，并在独立的临时环境中工作。
- 运行任何 GitHub Actions 工作流前，都必须由人工审批。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括对 MCP 服务器的访问。

## 审查生成的文档

先查看 GitHub Copilot cloud agent 创建的第一个拉取请求（PR），其中为代码添加了文档。你将使用 GitHub.com 上的标准 PR 界面。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。你可以[自定义或禁用 Copilot cloud agent 的防火墙][agent-firewall]。

1. 返回 github.com 上的仓库。
2. 选择 **Pull Requests** 查看 PR 列表。
3. 打开标题类似 **Add missing documentation** 或更完整的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]** 标记。请等待 Copilot 完成工作，这可能需要几分钟。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新增的 TSDoc 文档注释和其他文档。具体更改可能有所不同。检查其中是否包含 TSDoc 注释、TypeScript 规范和文件头注释；这些内容来自你在练习 2 开始时查看的自定义指令文件。
6. 审查完毕且确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待审批的工作流提示。
8. 如果有，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会进入 PR 的检查队列。如果一切正常，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 更改

你不必只单向地与 Copilot 协作。你可以像提及团队成员一样，在 PR 评论或代码行内评论中提及 Copilot。它会读取评论并启动新会话来处理请求。由于结果具有非确定性，我们无法规定你必须提出什么要求。可以让 Copilot 更新以下内容：

- 为每个代码文件顶部添加简要说明用途的注释。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用的结构。

1. 在评论中使用 **@copilot** 提及 Copilot，并要求它更改生成的文档。可以使用上面的任一建议，也可以提出其他文档改进要求。
2. 选择 **View Session** 查看 Copilot 执行工作。留意 Copilot 如何启动新会话来进行更新。
3. 选择 **Back to pull request** 返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中会出现新的提交。
5. 选择 **Files changed** 选项卡审查更改。

你可以继续迭代，直到满意为止。确认满意后，可将 PR 从草稿状态改为就绪，然后合并到 main 分支。

![将 PR 转为就绪状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

接下来查看 Copilot 为“在游戏详情页显示相关游戏” issue 创建的 PR。

1. 返回 GitHub.com 上的仓库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡审查生成的代码。
5. 审查完毕且确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有等待审批的工作流提示。
7. 如果有，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流应会进入 PR 的检查队列。如果一切正常，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。
9. **可选：**你可以在 codespace 中切换到该分支，手动测试相关游戏功能。打开 codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能！和之前一样，你可以通过 cloud agent 迭代改进成果，例如调整相关游戏的数量，或要求添加文件头注释和 TSDoc 文档注释（请记住，分配此任务时，你还没有更新自定义指令）。与之前一样，可以在 **Conversation** 选项卡中添加评论，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查通过无障碍自定义代理实现的功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 中在代理控制台要求添加的浅色模式。

1. 返回 GitHub.com 上的仓库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]** 标记。请等待 Copilot 完成工作，这可能需要几分钟。

4. 选择 **Files changed** 选项卡审查生成的代码。
5. 审查实现，特别留意：
   - 用于切换模式的 UI 组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。
6. 审查完毕且确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待审批的工作流提示。
8. 如果有，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会进入 PR 的检查队列。如果一切正常，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。
10. **可选：**你可以在 codespace 中切换到该分支，手动测试无障碍功能。打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换，并确认设置在重新加载页面后仍然保留。

留意无障碍自定义代理如何引导 Copilot 遵循无障碍最佳实践来实现这些功能。如果发现无障碍问题或改进点，可以像前面一样在评论中提及 **@copilot** 并要求更新。

## 可选练习：继续委派任务

Cloud agent 最适合处理真实的待办事项，让你能转而处理其他工作。可以在仓库中再创建几个 issue 并分配给 Copilot，逐步养成这种工作方式。以下是一些建议：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表页实现分页。
- 为数据访问 helper 添加输入验证和错误处理。

## 总结

你已完成 Cloud agent 实践环境。通过这些课程，你：

- **查看了仓库自带的自定义指令文件**，以便之后观察它们对 cloud agent 输出的影响。
- **将 issue 分配给 Copilot cloud agent**，并观察它设置环境、制定计划和异步执行任务。
- **创建并使用自定义代理**实现无障碍功能，添加高对比度和浅色模式切换。
- **使用 Copilot 代理页面作为控制中心**，监控并引导正在执行的无障碍会话。
- **审查并迭代改进 cloud agent 的拉取请求**，通过提及 `@copilot` 请求更改并审批工作流。

## 回顾和后续步骤

你已完成 Cloud agent 实践环境。还可以继续学习其他实践环境，巩固本课程的内容：

- 🖥️ **[VS Code 实践环境](../../vscode/)**：直接在 IDE 中探索 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 实践环境](../../cli/)**：在终端中完成两项经过审查的更改，体验 Plan 和 Autopilot 模式、代理技能、自定义 QA 代理、Playwright MCP、Agent Merge，以及可选的 `/delegate` cloud agent 桥接。

还可以在自己的仓库中尝试以下后续任务：

- 将重构或测试覆盖率 issue 分配给 cloud agent。
- 为其他领域创建自定义代理。
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [Copilot 代理简介][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理代理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
