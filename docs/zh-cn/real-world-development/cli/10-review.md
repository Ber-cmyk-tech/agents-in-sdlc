---
title: "第 10 课：总结和后续步骤"
description: "回顾 Copilot CLI 工作流程、两个拉取请求、可复用定制方式和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你使用 GitHub Copilot CLI 完成了 Tailspin Toys 的连续工作流程。你：

- 准备了 Codespace、安装 Copilot CLI、探索项目并找到预置的筛选 issue。
- 为游戏卡片添加星级评分，在转发浏览器中审查结果，并手动合并了第一个拉取请求（PR）。
- 从筛选 issue 开始，在 Plan 模式中确定方案、在 Autopilot 模式中实现，并在 Interactive 模式中审查。
- 使用自定义指令引导代理、自定义现有 `quality-checks` 技能，并使用它运行单元测试、lint 和类型检查。
- 添加 Playwright Model Context Protocol (MCP) 服务器，并使用它在真实浏览器中检查筛选功能。
- 创建并选择质量保证（QA）自定义代理，评估需求、覆盖率、技能结果和浏览器证据。
- 审查完整的筛选更改，并授权 Agent Merge 合并筛选 PR。
- 探索用于管理上下文、模型、共享和可选云端委派的斜杠命令。

## 你交付的内容

本工作坊有两个 PR 阶段，每个阶段都基于更新后的 `main` 创建独立分支：

1. **星级评分**：在游戏卡片上显示已有 `starRating`，并明确显示未评分状态。
2. **筛选和质量工作流**：实现筛选、更新指令并将其应用到功能、自定义 `quality-checks` 报告格式、创建 QA 配置文件，并包含相关测试。

从规划筛选功能到创建 PR，你始终使用同一对话和分支。为了简化工作坊，这些更改被整合到一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：运行自动化测试、亲自在浏览器中检查，以及通过 MCP 让 Copilot 在浏览器中探索。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 在创建 PR 前将这些结果与需求及测试覆盖率审查结合起来。

添加的测试应弥补实际缺口；QA 运行后不需要新增测试也可能是正确结果。授权合并前，请审查代码和证据；更改代码后，应重新生成受影响的验证证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。本工作坊中，你更新了指令、自定义了技能、创建了 QA 配置文件并配置了 MCP 服务器。可以在多个会话中复用这些定制方式，并根据团队需求调整。指令定义标准，技能描述可重复执行的任务，自定义代理定义专业角色，MCP 服务器连接外部工具。请审查实际更改和工具结果，而不只是代理的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 在实现前考虑方案，使用 **Interactive** 参与专注的更改，使用 **Autopilot** 处理范围明确的任务。常规编辑可选择更快的模型，复杂工作则可选择能力更强的模型。

上下文与基础设施同样重要。清晰说明要构建的内容、原因和方式，会显著影响输出。

## 更多可探索内容

你已完成核心工作流程。还可以了解一些 CLI 功能：

- `/review`：让代码审查代理分析更改。
- `/rubber-duck`：讨论问题并获得另一种观点。
- `/fleet`：并行协调独立子任务。
- `/worktree`：隔离单独任务。
- `/delegate`：将任务发送给 Copilot cloud agent。

## 后续步骤

要提升对任何工具的熟练度，最好的办法就是持续使用它！可以用它编写生产代码、兴趣项目，或你多年来一直想构建却还没开始的小应用。与团队分享经验，也向团队成员学习。别忘了继续探索文档。

如果想在终端中扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若想比较其他工作环境，可探索 [VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]或 [Copilot cloud agent 工作坊][cloud]。

## 资源

- [GitHub Copilot CLI 简介][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
