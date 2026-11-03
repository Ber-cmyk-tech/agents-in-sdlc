---
title: "练习 2：GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

许多组织都面临技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 的 cloud agent 可以自主执行更新代码、添加功能等任务。完成后，它会创建一个草稿 PR，供开发人员审查。这样可以将繁琐任务交给代理，加快开发进程，让开发人员专注于更重要的工作。

本课将使用 Copilot cloud agent：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 了解明确界定 issue 范围的重要性。
- 将 issue 分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受雇开发网站的承包商没有留下完善的文档，事实上完全没有文档。团队希望先为应用中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现体验。他们想在每款游戏的详情页显示同一类别中的其他游戏，让用户可以继续浏览。他们暂时不需要完善的实现，只需要能用于 UX 验收测试的版本。这项工作目前受到阻碍，但优先级低于其他 issue。

这类任务很容易被降级处理，正适合分配给 Copilot cloud agent。它可以异步处理任务，让开发人员先处理其他工作，之后再回来审查 Copilot 的成果并确认一切符合预期。

## 认识 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像开发人员一样在后台执行任务。你可以通过多种方式与它协作，例如[将 GitHub issue 分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求来跟踪进展、设置环境并开始处理任务。你可以在执行期间或完成后查看会话。解决方案准备好供你审查时，Copilot 会在拉取请求中提及你。

## 明确界定指令范围的重要性

GitHub Copilot 并非魔法，使用它不代表可以跳过思考。即使看似简单的操作，也可能包含复杂细节，因此[为 Copilot cloud agent 明确界定任务范围][cloud-agent-best-practices]非常重要。把它当作 AI 结对编程助手：分阶段工作、学习、试验并逐步调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本仓库中的自定义指令

前面的练习介绍了自定义指令及其作用。如果你已完成自定义指令实践，就见过 `.github/copilot-instructions.md` 的效果；如果还没有，现在可以快速查看。分配任务前，先只读查看仓库中的指令文件，稍后便能观察其影响。

在 GitHub 网页界面或已启动的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 规范和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将指令限定在 `**/*.test.ts` 文件中。

下一部分会将 `Code lacks documentation` issue 分配给 cloud agent。请留意生成的拉取请求中是否包含 TSDoc 文档注释、TypeScript 规范和文件头注释；这些内容来自指令文件。后续练习审查 PR 时还会再次提及。

## 为 Copilot cloud agent 设置开发环境

开发代码通常需要特定环境并运行一些设置脚本，以确保环境正常。将任务分配给 Copilot 时也是如此，因为它会以类似软件工程师的方式工作。

Cloud agent 使用 [GitHub Actions][github-actions] 执行任务。你可以创建[专用设置工作流][setup-workflow] `.github/workflows/copilot-setup-steps.yml`，在它开始工作前自定义环境。这样就能提供所需的开发工具和依赖项。为了让本实验顺利进行，该工作流已预先配置，确保 Copilot 可以使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及为单体 Astro 应用迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
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

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

这看起来像其他 GitHub 工作流文件，但有几个关键点：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- 留意 `workflow_dispatch` 触发器。它允许你从仓库的 **Actions** 选项卡手动运行工作流，这样可以测试工作流是否成功，而不必等待 Copilot 触发它。

## 添加文档

大家都知道文档很重要，但多数项目中的文档不是过时，就是根本不存在。这种技术债务常被忽视，降低工作效率，也增加维护代码库或引入新团队成员的难度。Copilot 擅长编写文档，因此很适合把这项工作分配给 Copilot cloud agent。它会在后台生成所需文档，后续练习中你将回来审查成果。

1. 在新的浏览器选项卡中打开 github.com 上的仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新 issue 对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

  ![将 issue 分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求（PR），其标题类似 `[WIP]: Code lacks documentation`。如果列表中还没有新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会在 PR 中创建待办列表。

> [!NOTE]
> Copilot 可能需要几分钟才会在 PR 中显示待办列表。它需要创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 查看 Copilot 准备完成的任务列表。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理 issue 的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态指示器。

16. 浏览实时会话，观察 Copilot 如何探索代码、了解当前状态、暂停思考并决定计划，以及编写代码。

这可能需要几分钟。Copilot cloud agent 的主要目标之一是异步执行任务，让我们能处理其他工作。现在就利用这一点：再分配一个任务给 Copilot cloud agent，然后开始编写应用功能代码。

## 为游戏详情页添加相关游戏

GitHub Copilot cloud agent 的一大优势是可以拆分工作：你处理一组任务时，它可以处理另一组。即使为设计团队添加相关游戏版块不需要太久，这段时间也可以用于其他工作。现在把它分配给 Copilot cloud agent。

1. 返回 github.com 上的仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新 issue 对话框。
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

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

  ![将 issue 分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后你会看到 issue 的第一条评论中出现一组 👀，表示 Copilot 已开始处理！

![Copilot 使用眼睛表情符号表示正在处理 issue](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二个请求。它的工作方式类似软件工程师，因此你不需要持续监控，等任务完成后再审查即可。现在可以继续创建并使用自定义代理。

## 总结和后续步骤

本课介绍了 [GitHub Copilot cloud agent][copilot-agents]。你可以将 issue 分配给它异步处理，借此解决技术债务、创建新功能，或协助将代码迁移到其他框架。

你学习了以下内容：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 了解明确界定 issue 范围的重要性。
- 将 issue 分配给 Copilot。

Copilot cloud agent 正在后台工作，现在可以开始创建和使用自定义代理。[Copilot cloud agent 也支持 MCP 服务器][cloud-agent-mcp]，并可使用自定义指令；我们已在前面的模块中介绍过这些内容。

## 资源

- [Copilot cloud agent 概述][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义代理 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
