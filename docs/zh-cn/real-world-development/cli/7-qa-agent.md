---
title: "第 7 课：创建并使用 QA 代理"
description: "创建一个 QA 自定义代理，结合 issue 要求、quality-checks 技能和 Playwright MCP。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，并使用 Playwright MCP 在浏览器中检查筛选体验。现在将这些能力整合到一个具有明确 QA 流程的自定义代理中。

本课将学习如何：

- 了解自定义代理如何结合指令、技能和 MCP 工具。
- 创建并检查可复用的质量保证配置。
- 选择 QA 代理，根据筛选 issue 审查结果。

## 场景

Tailspin Toys 希望在创建拉取请求（PR）前，以一致的方式审查需求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义代理可以协调 QA 流程，并生成可复用的报告。

## 什么是自定义代理？

自定义代理是通过 Markdown 配置文件定义的 Copilot 专用版本。配置文件说明代理的用途、指令和可用工具。本工作坊将把 QA 角色定义在 `.github/agents/qa.agent.md` 中，并在 Copilot CLI 中选择它。

你使用过的定制功能各有用途：仓库指令描述团队标准；`quality-checks` 技能封装可重复运行的检查；Playwright MCP 提供浏览器工具；QA 配置文件则指导 Copilot 使用这些能力评估需求并报告发现。它不会取代这些功能，也不需要开始另一个对话。

## 创建 QA 配置文件

创建功能 PR 前，让 Copilot 创建一个可复用的 QA 配置文件，其中定义 QA 要执行的检查和必须遵守的边界：

1. 返回 Codespace，并确认筛选对话处于 Interactive 模式。
2. 要求默认代理创建新的自定义代理：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新代理前，先确认 Copilot 捕获了预期的 QA 流程和边界，避免代理不完整或范围过宽，在你只想验证功能时修改实现代码。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 查看 frontmatter。`description` 为必填项；`name` 可选，但添加后代理会有明确的显示名称。
3. 阅读配置文件指令，确认 QA 会从需求出发、遵循仓库指令、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持性证据、修改实现代码前会先询问，而且不会提交更改或创建拉取请求。
5. 如果生成的配置缺少任何职责或边界，先要求默认代理修改，再继续。

## 根据 issue 运行 QA

Copilot CLI 会在对话开始时加载项目代理。创建配置文件后，恢复同一筛选对话并选择 QA，以便它使用已有的 issue 和规划决定。

1. 使用以下提示启用代理：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的代理可能暂时不会出现在可用代理列表中，但仍可以使用上述命令启用。

2. 使用以下提示让 QA 代理审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 代理将开始工作。
4. 完成后阅读它生成的报告。

## 总结和后续步骤

你添加了一个可复用的专业角色并审查了它的工作。本课中，你：

- 了解了自定义代理如何结合指令、技能和 MCP 工具。
- 创建并检查了可复用的质量保证配置。
- 选择 QA 代理，根据筛选 issue 审查结果。

现在，功能实现、技能更新、QA 配置文件、测试和验证报告都已准备好供审查。下一步，[创建功能 PR 并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义代理][create-agents]
- [自定义代理配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
