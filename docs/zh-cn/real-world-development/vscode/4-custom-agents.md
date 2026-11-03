---
title: "练习 4：自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]是针对开发工作流中的特定任务或领域定制的 AI 助手。你可以在仓库的 `.github/agents` 文件夹中创建 Markdown 文件来定义代理，为它提供专门的指令、最佳实践、编码模式和领域知识，使它更有效地完成特定类型的工作。团队可以将专业知识编写成可复用的代理，例如确保符合 [WCAG][wcag] 标准的无障碍代理、遵循安全编码实践的安全代理，或保持测试模式一致的测试代理。

自定义代理由项目 `.github/agents` 文件夹或全局 `~/.copilot/agents` 文件夹中的 Markdown 文件定义。每个文件都包含 YAML frontmatter，至少指定 `name` 和 `description`，后面是用于定义代理行为、专业知识和指令的 Markdown 提示。

### 自定义代理与代理技能

自定义代理与[代理技能][agent-skills-concept]有些相似。两者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义代理是执行工作的人员，技能是工具**。

自定义代理拥有独立的上下文窗口，可在工作中编排技能甚至其他代理。本实验中的无障碍自定义代理会依据无障碍指南审查并更新网站；在执行过程中，它可以调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 编写自定义代理没有唯一的正确方式。与 AI 的其他用法一样，需要在自己的环境和场景中测试并迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，当前暗色主题的文字与背景对比度不足，导致部分用户难以阅读。为解决此无障碍问题，设计团队要求实现可由用户切换的高对比度模式。

无障碍十分重要，因此需要尽快实现此功能。你将使用自定义代理生成该功能。

本练习将学习如何：

- 查看现有的无障碍自定义代理。
- 在 Copilot Chat 中使用无障碍代理实现高对比度模式。

## 查看无障碍自定义代理

项目已为无障碍任务创建了自定义代理。返回 codespace，然后打开新分支以单独处理无障碍工作（练习 3 中的筛选 PR 会保持独立）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含 `name` 和 `description` frontmatter。

3. 浏览后续部分，留意其中包含：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见问题和错误列表。

## 在 Copilot Chat 中使用自定义代理

VS Code 会在 Copilot Chat 视图底部的代理下拉菜单中显示 `.github/agents` 中定义的所有自定义代理。选择代理后，聊天会话将使用该代理的指令和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的会话。模式和模型选择因练习而异；每个步骤会在适用时说明。

1. 如果代理下拉菜单中尚未选择 **Agent**，请先选择它。

   ![Chat 视图中的代理选择器](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的代理下拉菜单（默认显示当前代理 **default**）。
3. 从代理列表中选择 **Accessibility agent**。
4. 向无障碍代理发送以下提示：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 将开始处理任务：探索代码库、提出更改建议并编辑项目文件。每项编辑都会以内联差异的形式显示在聊天中，并包含文件路径，供你审查。
6. 代理修改文件时，**Files changed** 指示器会更新，显示本会话中已修改的文件。

> [!NOTE]
> 这可能需要几分钟。Copilot 会实际修改仓库文件，例如 Astro 组件、CSS 和相关测试。下一练习中，你将审查并引导这项进行中的工作。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 自定义代理][custom-agents]，也就是针对特定任务和领域定制的 AI 助手。使用自定义代理可以将团队的专业知识和标准编写成可复用的代理，指导 Copilot 更有效地完成特定类型的工作。

你学习了以下内容：

- 如何定义自定义代理。
- 如何在 Copilot Chat 代理模式中使用自定义代理。

接下来，[监控并引导代理工作][next-lesson]，在更改生成过程中进行审查，并要求它在同一会话中添加浅色模式切换。

## 资源

- [自定义代理简介][custom-agents]
- [在 IDE 中创建自定义代理][creating-custom-agents-ide]
- [VS Code 中的自定义代理][custom-agents-vscode]
- [自定义代理配置][custom-agents-config]
- [awesome-copilot 上的自定义代理][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
