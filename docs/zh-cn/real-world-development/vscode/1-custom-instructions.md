---
title: "练习 1：自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解背景信息，就要确保它能访问这些上下文。[指令文件][instruction-files]是提供代码生成指导的主要方式。

本练习将学习如何：

- 通过仓库自定义指令和按路径应用的指令文件，为 Copilot 提供项目上下文、编码指南和文档规范。
- 使用当前指令生成第一个筛选数据切片（发行商数据访问 helper）。
- 在 `.github/copilot-instructions.md` 中添加新的仓库级标准。
- 重新运行相同提示，观察生成的代码如何采用新标准。
- 将指令更新和筛选功能切片提交到 `main`，供下一练习中的 Copilot 使用。

> [!CAUTION]
> 生成的代码可能不完全符合你设定的某些标准。Copilot 具有非确定性。本练习旨在观察更新指令前后行为的变化趋势，而不是要求输出逐字一致。

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

- `.github/copilot-instructions.md`：每次针对该仓库的请求都会发送给 Copilot 的单个指令文件。适合记录大多数请求都适用的项目级信息。
- `.github/instructions/*.instructions.md`：可针对特定语言、文件类型或任务提供指引。

> [!NOTE]
> 在 IDE 中，指令文件只用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 生成代码时，都会使用仓库级指令文件和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

示例项目展示了一种有代表性的做法：

- 在 `copilot-instructions.md` 中记录项目简介、结构和全局编码标准等项目级指导。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用清晰的自然语言，并提供代码的正反示例。

创建指令文件没有唯一的方法，使用 AI 也是如此。通过试验找到适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一套完善的指令文件。查看本项目时，你会发现其中包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]等任务的指令。
>
> Copilot 也可以帮助生成指令文件。不同界面提供的入口有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。本工作坊会在相关课程中介绍这些功能。
>
> 需要模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中有大量指令文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义指令文件

先查看入门项目中的指令文件，再进行更改：

1. 在 codespace 中打开 `.github/copilot-instructions.md`。
2. 查看项目简介和编码指南。这些指令适用于 Copilot 在此仓库中的每次交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。你会看到适用于 Astro 文件、Drizzle 数据层、测试等方面的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它设置适用文件的 glob。
5. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向的其他指令文件和项目现有文件。这些引用有助于拆分和复用复杂指令，并为 Copilot 提供示例。

## 为更改创建分支

遵循最佳实践，为更改创建分支：

1. 返回上一练习中的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的会话。模式和模型选择因练习而异；每个步骤会在适用时说明。

先在当前指令下发送提示，再更新指令并重新发送相同提示，以观察自定义指令的效果。

1. 关闭之前练习留下的编辑器选项卡，避免 Copilot 获取不需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道 helper 应位于何处。
3. 在 Chat 视图的代理下拉菜单中选择 **Agent**，让 Copilot 能够应用文件更改。

   ![Chat 视图中的代理选择器](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改 helper 文件及其测试。
6. 留意生成的 helper 是类型化函数：第一个参数为 `db` 客户端，返回类型化的发行商数组。这遵循 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层规范。
7. 留意生成的代码**缺少** TSDoc 文档注释和文件头注释。

> [!CAUTION]
> Copilot 具有非确定性，因此即使没有明确要求，也可能添加文档注释。如果发生这种情况也没关系，本练习重点是观察更新指令后的一致性改进。

## 添加新的仓库标准

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。添加编码标准可以改善代码建议：

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到靠近第 27 行的 **Code formatting requirements** 部分。它记录了项目编码标准，但还没有代码文档规则，因此生成的 helper 没有文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件头注释和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可在仓库级别（`.github/copilot-instructions.md`）创建全局指令，也可使用 `*.instructions.md` 为特定语言、文件类型或任务提供指引。仓库级文件适合存放刚添加的文档注释等项目级标准。

## 重新运行提示并观察更改

1. 返回 Copilot Chat 并选择 **New Chat** 清除会话内容。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦正确的文件。
3. 发送之前的**相同提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 查看新生成的文件是否以类似以下注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 查看生成的函数是否现在带有类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚才引导 Copilot 遵循新的项目标准，并将其应用到下一练习将继续使用的代码中。

## 提交指令并推送分支

指令文件和其他资源一样都由源代码管理。接下来提交并推送分支：

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认指令和 helper 更改均已存在：

   ```bash
   git status
   ```

3. 暂存并提交指令更改，以及 Copilot 为 helper 基础实现更改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到仓库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，创建拉取请求并关联到待办列表中的文档标准 issue。可以手动操作，也可以让 Copilot 使用已连接到项目的 GitHub 工具。让它查找 issue、创建并关闭该 issue 的 PR，然后合并 PR。

> [!NOTE]
> 这是首次让 Copilot 代表你操作 GitHub。项目模板已配置好 GitHub 工具，无需额外设置。首次使用时，VS Code 可能会提示你登录 GitHub。按提示授权即可。后续课程会介绍此连接的工作方式（Model Context Protocol）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保在代理选择器中选择了 **Agent**。
3. 要求 Copilot 查找与更新指令文件有关的 issue，并从当前分支创建 PR，说明指令更新和发行商 helper 基础实现：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找 issue 并创建 PR。
5. 当 Copilot 提示允许其代表你执行 GitHub 操作时，检查操作并按需选择 **Allow**。
6. PR 创建后，要求 Copilot 合并 PR 并将当前分支切回 main：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 当 Copilot 请求执行 GitHub 操作和 shell 命令的权限时，先检查命令，再按需选择 **Allow**。

现在，你已借助 GitHub Copilot 创建并合并了拉取请求！

## 总结和后续步骤

你了解了 Copilot 如何读取本项目的指令文件，并在 VS Code 中使用 Copilot Chat 完成以下任务：

- 发送代码生成提示，观察 Copilot 在现有指令下的输出。
- 在 `.github/copilot-instructions.md` 中添加新的仓库级标准。
- 重新发送相同提示，观察生成的代码如何遵循新标准。
- 将指令更新和 helper 基础实现提交到 `main`，供下一练习使用。

接下来，在[代理模式][next-lesson]中运用这些指令，让 Copilot 跨代码库添加新功能。

## 资源

- [GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot：指令文件及其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
