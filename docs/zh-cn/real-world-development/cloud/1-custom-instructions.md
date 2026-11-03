---
title: "练习 1：自定义指令（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot cloud agent →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解背景信息，就要确保它能访问这些上下文。[指令文件][instruction-files]可以提供这些指导，让 Copilot 不仅了解你要完成什么，也了解应该如何完成。

本练习将学习如何：

- 通过仓库自定义指令和按路径应用的指令文件，为 Copilot 提供项目上下文、编码指南和文档规范。
- 在 `.github/copilot-instructions.md` 中添加一条新的仓库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 实践环境不同，这里不会运行前后对比提示。Copilot cloud agent 会异步处理 GitHub issue，因此难以实时并排展示指令的影响。稍后审查 cloud agent 创建的拉取请求时，你会看到这些指令的作用。

## 指令文件

### 场景

Tailspin Toys 制定了一些开发实践指南和要求：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加一段注释，说明文件的用途。

使用指令文件可确保 Copilot 了解这些实践，并据此完成任务。

### 自定义指令

自定义指令可以向 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。你可以指定编码规范、库以及希望包含的注释类型。可以为整个仓库创建指令，也可以为特定文件类型提供任务级上下文。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该仓库的请求都会发送给 Copilot 的单个指令文件。适合记录大多数 Copilot Chat 或 CLI 请求都适用的项目级信息，例如技术栈、项目概况、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建的文件，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，指令文件只用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 生成代码时，都会使用仓库级指令文件和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本工作坊的范围。示例项目展示了一种有代表性的做法：

- 在 `copilot-instructions.md` 中集中记录项目级指导，例如项目简介、结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，保持说明清楚，并提供正反示例。

创建指令文件没有唯一的方法，使用 AI 也是如此。通过试验找到适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一套完善的指令文件。查看本项目中的文件时，你会发现其中包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]等任务的指令。
>
> Copilot 也可以帮助生成指令文件。不同界面提供的入口有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。本工作坊会在相关课程中介绍这些功能。
>
> 需要模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中有大量指令文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义指令文件

阅读仓库自带的指令文件：一个核心 `copilot-instructions.md`，以及一组适用于不同任务的 `*.instructions.md` 文件。你可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。特别留意 **Code standards** 中嵌套的 **GitHub Actions Workflows** 指南。这些内容适用于你与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的内容。你会看到适用于 Astro 文件、Drizzle 数据层、测试等方面的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它设置了相对于仓库根目录的 glob，用于确定适用文件。此处匹配任何 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 查看为本项目编写单元测试的专用指令。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。留意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成更小、可复用的文件，并为 Copilot 提供代码示例。（这些路径相对于指令文件，而不是仓库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求在代码中添加文档注释。接下来，你会添加 TSDoc 文档注释和文件头注释的规则。

## 添加新的仓库标准

接下来要做的唯一编辑，是添加一条项目级规则，要求在代码中使用 TSDoc 文档注释和文件头注释。之后 cloud agent 处理本实践环境中的 issue 时会采用这条规则。

编辑前先创建分支（cloud agent 会读取 issue 目标分支中的指令，但仍建议干净地提交更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。添加编码标准可以改善代码建议：

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到靠近第 27 行的 **Code formatting requirements** 部分。它记录了项目编码标准，但还没有代码文档规则，因此生成的 helper 没有文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件头注释和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可在仓库级别（`.github/copilot-instructions.md`）创建全局指令，也可使用 `*.instructions.md` 为特定语言、文件类型或任务提供指令。项目级文件适合存放刚添加的文档注释等全局标准。

## 提交、推送并合并指令更新

Cloud agent 会读取 issue 目标分支中的指令。下一练习中，你会将 issue 分配给 Copilot，而它会从 `main` 创建分支。因此，必须先将指令更改合并到 `main`，cloud agent 才能使用它们。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。之后 cloud agent 处理下一练习中分配给它的 issue 时就会读取这些指令。

> [!TIP]
> 如果希望在本工作坊中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。这里保留分支步骤是为了模拟真实项目的工作方式。

## 总结和后续步骤

你了解了 Copilot 如何读取本项目中的指令文件，并向 `.github/copilot-instructions.md` 添加了新的仓库级标准。后续练习中，你会在 cloud agent 创建的拉取请求中看到该标准的效果。

接下来，[将第一个 issue 分配给 Copilot cloud agent][next-lesson]。

## 资源

- [GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件及其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
