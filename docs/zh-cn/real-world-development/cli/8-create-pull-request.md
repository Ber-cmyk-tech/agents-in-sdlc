---
title: "第 8 课：创建并合并功能 PR"
description: "一起审查筛选功能、指令、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、指令更新、技能更新、质量保证（QA）配置文件和测试都已保存在同一分支上。现在一起审查它们并创建拉取请求。你之前手动合并了星级评分 PR；这次将使用 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常会将功能、指令更新、技能更新和 QA 代理拆分到几个独立的 PR 中。为简化本工作坊，我们将筛选功能和质量工作都保存在同一对话和分支中，并放入同一个 PR。

本课将学习如何：

- 了解 Agent Merge 以及它如何自动化 PR 合并生命周期。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 审查完成后启用 Agent Merge，并确认 PR 已合并。

## 场景

在筛选工作流程中，你使用 Copilot 规划、实现并验证了一项功能。Tailspin Toys 现在希望自动化 PR 的后续工作，同时由开发人员控制合并授权。

## 认识 Agent Merge

**Agent Merge** 会自动处理合并拉取请求所需的剩余工作。启用后，Copilot 会处理阻碍 PR 合并的问题，例如修复失败的持续集成 (CI) 检查、回复审查评论，以及在需要时进行变基；随后，在仓库允许的情况下启用 GitHub 自动合并。

此前你都是自己选择 **Merge pull request**。Agent Merge 可以代劳，但启用前仍要先审查工作并确认 PR 已准备就绪。

## 使用 Agent Merge 管理 PR

代码都已完成，现在一起审查、创建 PR，并让 Agent Merge 处理其余流程：

1. 返回 Codespace。
2. 输入 `/agent` 打开代理对话框。
3. 从选项中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 输入 `/pr create` 创建新的 PR。
5. 输入 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程。所有检查通过后，它会执行合并。
7. 确认 Copilot 显示类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需审批、分支保护、合并队列、仓库设置或权限限制。如果操作受阻，请阅读报告的原因；在仓库允许的情况下，完成审查后手动合并。

## 总结和后续步骤

你已自动化开发流程中的多个部分，包括生成代码、测试和验证代码，以及管理拉取请求。本课中，你：

- 了解了 Agent Merge 以及它如何自动化 PR 合并生命周期。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 仅在审查完成后启用了 Agent Merge，并确认 PR 已合并。

接下来，[探索更多有用的 Copilot CLI 斜杠命令][next-lesson]，管理上下文、模型、共享和可选的云端委派。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
