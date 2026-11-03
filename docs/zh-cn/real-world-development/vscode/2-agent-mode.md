---
title: "练习 2：使用 Copilot 代理模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用更新，通常也需要修改多个文件并执行测试。开发人员往往要查找所有相关文件、进行更改、运行测试、调试、发现遗漏的文件，再继续修改。这些工作可能会不断重复。

Copilot 代理模式可以帮助处理这些任务。

Copilot 代理模式可以在 IDE 中更自主地工作。它会先探索现有项目结构，进行必要的更新，运行测试等任务，并自动修复发现的问题。接下来看看如何使用代理模式为网站添加新功能。

> [!NOTE]
> 代理模式和 cloud agent 名称相似，但适用于不同的体验。代理模式在 IDE 中执行任务，便于快速反馈和交互。Cloud agent 则像团队成员一样异步工作，通过 issue 和拉取请求与你协作。

本练习将学习如何：

- 让 Copilot 代理模式探索项目、查找相关文件并协同修改。
- 使用 GitHub Copilot 代理模式跨 UI 和数据层实现新功能。
- 在将更改合并到代码库前审查 Copilot 代理模式生成的更改和测试。

## 场景

游戏目录不断扩充，你想允许用户按类别和发行商筛选。上一练习已添加发行商数据访问 helper，现在将使用 Copilot 代理模式完成其余数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

更改网站前，先了解当前功能。

该网站是一个面向开发者的棋盘游戏众筹平台，可列出游戏并显示详情。它是一个单体 Astro 应用，页面在构建时渲染为静态 HTML。页面在 frontmatter 中通过 Drizzle ORM 直接查询本地 SQLite 数据库，没有独立的后端 API 或客户端 UI 框架。可复用的数据访问 helper 位于 `src/lib/`，交互功能由使用标准 DOM API 的小型 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。请在 GitHub Codespace 中按以下步骤操作：

1. 返回 codespace。上一练习结束后，你应已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开终端。
3. 创建并切换到筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下 URL 的提示：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出窗口询问是否打开 `http://localhost:4321`，选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中，选择终端中的 localhost URL 会自动转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到 codespace（现已托管 Web 服务器）的私有隧道。

### 浏览网站

网站启动后，可以浏览其功能：

- **主页**：显示棋盘游戏列表，包括标题、图像和说明。
- **游戏详情页**：选择游戏后会打开详情页，显示标题、说明、发行商和类别等信息。

## 使用 Copilot 查看待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的会话。模式和模型选择因练习而异；每个步骤会在适用时说明。

网站的初始实现已能正常工作，但我们希望添加新功能。先查看待办列表。根据模板设置项目时，系统已自动创建一组 issue；让 GitHub Copilot 显示这些项目。

1. 在 Chat 视图的代理下拉菜单中选择 **Agent**。Agent 可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的代理选择器](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并未偏好某个模型。编写本工作坊时使用了 Claude Sonnet 4.5，因此将其列入说明，以便获得较一致的体验。但由于 LLM 具有非确定性，你可能会看到与说明不同的建议，这是正常现象。

> [!NOTE]
> 由于 LLM 具有非确定性，Copilot 可能使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问待办 issue：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 以运行列出所有 issue 的命令。
5. 查看生成的 issue 列表。

留意 Copilot 还会根据它认为对用户最有用的程度为项目排序。

## 查看指令文件

让代理生成代码前，先查看用于提供上下文的指令文件。本练习将使用 [UI 指令文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中包含向网站添加功能的指导。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其总体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向各类文件专用指令的链接，包括 Astro、Tailwind CSS 和 Drizzle。

> [!TIP]
> 指令文件可引用其他指令文件和项目文件。路径相对于指令文件所在位置，因此可以复用内容、拆分复杂指导，并为 Copilot 提供模板和示例。

## 实现筛选功能

完成筛选至少需要对应用做三项更新：

- 在数据层（`src/lib/`）添加或完善筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选 UI。

合并前还必须运行并通过测试。Copilot 代理模式可以执行这些任务：

1. 继续当前 Copilot 会话，或选择 **New Chat** 开始新会话。
2. 选择 **Add Context**、**Instructions**，然后选择 `ui` 指令文件。

   ![选择 UI 指令文件示例](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图中仍选中 **Agent**。

   ![Chat 视图中的代理选择器](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择模型 **Claude Sonnet 4.5**。
5. 使用待办列表中的相关 issue 提示 Copilot 实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何探索项目、查找相关文件，包括数据层 helper、UI 和测试，然后修改文件并运行测试。

   ![Copilot 探索项目文件](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。过程中编辑器可能会显示 **Keep** 和 **Undo** 按钮。工作完成后，每项更改都会显示这两个按钮，因此无需在执行过程中提前选择。

7. 按 Copilot 提示选择 **Continue** 运行测试。

   ![Copilot Chat 提示用户确认运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，甚至出现测试失败。这没有关系！Copilot 会在代码生成和测试之间反复迭代，直到完成任务且未检测到错误。

   ![Copilot 代理模式完整会话](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在潜在问题。

> [!CAUTION]
> 始终要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，试用新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

本练习中，你使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加了功能。你学习了如何：

- 使用 GitHub Copilot 代理模式跨 UI 和数据层实现新功能。
- 让 Copilot 代理模式探索项目、查找相关文件并协同修改。
- 在将更改合并到代码库前审查生成的更改和测试。

接下来，[使用 Playwright MCP 服务器测试功能][next-lesson]并创建拉取请求。

### 附加探索练习：实现分页

随着游戏列表不断增长，需要添加分页。运用本练习学到的技能，提示 Copilot 更新网站以实现分页。编写代码时可考虑：

- 遵循现有最佳实践，包括使用已有指令文件。
- 确定分页方式，例如是否允许用户选择页面大小，还是使用固定页面大小。
- 在提示中提供必要指导，确保 Copilot 按期望实现功能。
- 可能需要与 GitHub Copilot 多次迭代，提出更改请求并提供上下文。这是正常的协作流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
