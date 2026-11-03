---
title: "练习 3：使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：代理模式][previous-lesson] · [下一课：自定义代理 →][next-lesson]

你刚才已使用代理模式构建筛选功能。在创建拉取请求前，应先确认它在浏览器中正常工作。你将连接 **Playwright MCP 服务器**，让 Copilot 操作真实浏览器并测试功能，然后发布分支并创建 PR。

本练习将学习如何：

- 了解 Model Context Protocol (MCP) 以及 MCP 服务器如何为 Copilot 扩展工具。
- 将 Playwright MCP 服务器添加到工作区。
- 让 Copilot 使用它在浏览器中手动测试筛选功能。
- 发布分支并为筛选工作创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

当代理模式能够访问编辑器以外的工具时，功能会大幅增强。Copilot 使用 Model Context Protocol (MCP) 与外部工具和服务通信。

![代理模式的内部工作方式示意图，展示其与上下文、LLM 和工具的交互，包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 代理与外部工具和服务提供了通信方式。AI 代理可通过 MCP 使用资源获取最新信息，并使用工具代表你执行操作。

MCP 服务器充当代理与外部工具和服务之间的桥梁，负责管理代理与外部工具（例如现有 API 或 npm 包等本地工具）之间的通信。每个 MCP 服务器都提供一组代理可访问的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供用于管理 GitHub 仓库、issue 和拉取请求的 API。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力，让代理可以访问网页、填写表单和选择按钮。

还有许多其他 MCP 服务器可访问不同工具和资源。GitHub 提供 [MCP 注册表](https://github.com/mcp)，帮助用户发现并为生态系统做出贡献。

> [!CAUTION]
> 应像对待项目的其他依赖项一样对待 MCP 服务器。使用前先审查源代码、验证发布者并考虑安全影响。只使用可信的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区中可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 你会看到已配置好的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` 条目随项目模板提供，因此 Copilot 能在上一练习中读取待办 issue。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装；VS Code 通过 HTTP 连接，并在 Copilot 首次使用其工具时使用 GitHub 身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的浏览器，用于测试功能。

1. 在 `.vscode/mcp.json` 中，在 `github` 旁边添加 `playwright` 条目，使文件内容如下：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是本地服务器：VS Code 会在本机运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 参数会让 Playwright 在后台运行浏览器，无需显示窗口；codespace 中没有可显示窗口的桌面环境，因此必须使用此参数。

> [!NOTE]
> Tailspin Toys 项目已经使用 Playwright 运行端到端测试，因此通常已安装 Playwright 所需的浏览器。如果 Copilot 报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 按需启动 MCP 服务器：Copilot 首次需要使用服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. `.vscode/mcp.json` 中的 `playwright` 条目上方会出现 **Start** 操作。选择它以立即启动服务器并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请检查配置并选择信任，以便启动服务器。

手动启动是可选的。如果跳过，VS Code 会在下一步中 Copilot 首次需要使用其工具时启动服务器，并询问是否信任。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可操作的真实浏览器。代理可以打开页面、导航、应用筛选并读取结果，然后向你总结观察结果。这样无需离开会话就能快速确认功能是否符合预期。

Playwright MCP 服务器使用页面的[无障碍树][playwright-mcp-server]而不是屏幕截图。代理会基于带标签的结构化元素（按钮、链接、列表项）进行推理，与辅助技术的工作方式类似，因此简单的功能检查也能兼顾基本的无障碍检查。

服务器已连接且应用正在运行时，向 Copilot 发送以下提示，让它测试刚实现的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器、执行各步骤并汇报结果。根据 issue 中的验收标准审查总结；如果发现异常，可以继续提问或让 Copilot 修复代码，再创建拉取请求。

> [!NOTE]
> 测试时，应用必须运行在 `http://localhost:4321`。如果开发服务器已停止，请在发送提示前重新启动。如果 Copilot 首次使用 Playwright MCP 服务器时需要下载浏览器，请让它运行 `npx playwright install chromium`，然后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求（PR），让团队审查。首先发布 `filtering-vscode` 分支：

1. 在 codespace 中打开 **Source Control** 面板，审查 Copilot 的更改。
2. 选择 **+** 暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示 Copilot 更改的 Source Control 面板](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish** 将分支推送到仓库。

## 创建拉取请求

可以通过 github.com 或 GitHub 命令行界面 (CLI) 创建拉取请求。也可以让 GitHub Copilot 帮忙查找相关 issue 并创建关联 PR：

1. 打开 Copilot Chat 面板并选择 **New Chat**，开始新会话。
2. 确认选择了 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 发送以下提示：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 按需选择 **Continue**，允许 Copilot 收集信息并执行操作。首次使用 GitHub 工具时，可能会提示你进行 GitHub 身份验证，请按提示授权。
5. 留意 Copilot 如何搜索 issue、找到正确的 issue 并创建 PR。
6. 选择 Copilot 返回的链接来审查拉取请求，但**暂时不要合并**。

## 总结和后续步骤

本练习中，你：

- 了解了 Model Context Protocol (MCP) 以及 MCP 服务器如何为 Copilot 扩展工具。
- 将 Playwright MCP 服务器添加到工作区。
- 使用它在浏览器中手动测试筛选功能。
- 发布分支并为筛选工作创建拉取请求。

你使用 MCP 服务器测试了一项功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义代理][next-lesson]，以便高效完成无障碍审查等专门任务。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP 服务器][playwright-mcp-server]
- [GitHub MCP 服务器][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
