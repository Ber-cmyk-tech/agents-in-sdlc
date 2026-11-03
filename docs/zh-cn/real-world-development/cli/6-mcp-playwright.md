---
title: "第 6 课：使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中查看或配置 Playwright MCP，并用它在浏览器中探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

正如前面提到的，编写代码不只是写代码。我们还需要处理数据、外部服务，并允许 Copilot 使用更多自动化工具。MCP 服务器可以让 Copilot 超越 CLI 内置功能，访问更多工具和服务。

本课将学习如何：

- 了解 Model Context Protocol (MCP) 以及 Copilot CLI 如何使用它。
- 如果尚不可用，则添加 Playwright MCP 服务器。
- 让代理操作浏览器并探索筛选功能。

## 场景

单元测试和端到端测试固然重要，但验证 UI 更新还需要实际与 UI 交互。你希望 Copilot 能像用户一样使用正在开发的网站，进一步自动化更改验证过程，并提高对预期行为的信心。

## 什么是 Model Context Protocol (MCP)？

[Model Context Protocol (MCP)][mcp-blog-post] 为 AI 代理与外部工具和服务提供了通信方式。AI 代理可以通过 MCP 实时与外部工具和服务通信，访问最新信息并代表你执行操作。

MCP 服务器充当代理与外部工具和服务之间的桥梁。每个 MCP 服务器都提供一组代理可访问的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 仓库、issue 和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化能力。

还有许多其他 MCP 服务器。GitHub 提供 [MCP 注册表][mcp-registry]，帮助用户发现并为生态系统做出贡献。

> [!CAUTION]
> 应像对待项目的其他依赖项一样对待 MCP 服务器。使用前先审查源代码、验证发布者并考虑安全影响。

## 添加 Playwright MCP 服务器

如果 Copilot CLI 中尚未配置 Playwright MCP 服务器，请添加它，使 Copilot 能像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 中已经配置了 Playwright MCP，请跳过这些设置步骤，继续下一部分。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 名称输入 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 探索功能

之前你已手动确认功能按预期运行。现在使用 Copilot 可访问的 Playwright MCP 服务器重复验证：

1. 使用以下提示，要求 Copilot 通过 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常 Copilot 会自动找到 MCP 服务器，不一定需要明确要求它使用该服务器。但提供提示有助于它选对工具，让结果更一致，也能节省一些 token。

2. 观察 Copilot 如何在浏览器中执行各项步骤，确认功能是否正常。
3. 阅读报告并确认行为符合预期。

Copilot 会启动服务器、使用 Playwright 操作网站、停止服务器并生成报告。

## 总结和后续步骤

恭喜，你已在 Copilot CLI 中使用 Playwright MCP 服务器通过真实浏览器探索功能！本课中，你：

- 了解了 Model Context Protocol (MCP) 以及 Copilot CLI 如何使用它。
- 如果之前尚未配置，则添加了 Playwright MCP 服务器。
- 让代理操作浏览器并探索筛选功能。

接下来，[创建 QA 自定义代理][next-lesson]，将技能与浏览器工具结合起来。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP 服务器][playwright-mcp-server]
- [为 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
