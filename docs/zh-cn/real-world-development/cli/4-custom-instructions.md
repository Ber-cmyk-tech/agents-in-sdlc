---
title: "第 4 课：使用自定义指令指导 Copilot"
description: "查看仓库指令、添加文档标准，并将其应用到筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，就要确保 Copilot 能获取相应指导。[指令文件][instruction-files]不仅说明你希望代码实现什么，还能说明代码应如何组织。现在你已完成筛选功能，接下来将查看 Copilot 使用的指令、添加文档标准并将其应用到代码中。

本课将学习如何：

- 了解仓库指令和按路径应用的指令文件如何传递给代理。
- 更新指令文件，确保遵循编码标准。
- 观察指令文件对代码的影响。

## 场景

Tailspin Toys 制定了一些开发实践指南和要求：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中的导出函数应使用 TSDoc/JSDoc 说明用途、参数和返回值；如果有可注入的 `db` 参数，也应予以说明。
- 可复用的 Astro 组件应说明其 `Props` 接口；相关代码更改时，注释也应保持更新。
- 保留现有格式和 lint 指南。

使用指令文件可确保 Copilot 了解这些实践，并据此完成任务。

## 指令文件

自定义指令可以向 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。你可以指定编码规范、库以及希望包含的注释类型。可以为整个仓库创建指令，也可以为特定文件类型提供任务级上下文。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该仓库的请求都会发送给 Copilot 的单个指令文件。适合记录大多数请求都相关的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导的文件。

> [!NOTE]
> 其他指令格式及其支持情况因环境而异。使用特定格式前，请查阅[自定义指令支持参考][custom-instructions-support]。

## 查看项目中的自定义指令文件

入门项目已包含一组指令文件。先查看现有文件，再进行更改，以便观察效果：

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 查看项目简介和编码指南。这些指令适用于 Copilot 在此仓库中的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。你会看到适用于 Astro 文件、Drizzle 数据层、测试等方面的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它设置了指令适用文件的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向的其他指令文件和项目现有文件。这有助于拆分和复用指令，并为 Copilot 提供示例。

## 更新指令以符合团队规范

虽然现有文件已是很好的起点，但仍有缺口。修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释：

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分（大约第 35 行）。
2. 在最后一条项目符号后添加以下内容：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 使用更新后的指导

Copilot CLI 会在对话开始时加载仓库指令。保存更新后，退出并恢复筛选会话，以便 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 关闭它。Copilot CLI 会自动保存会话。
2. 在仓库根目录的 `game-filters-cli` 分支中恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认检出的是 `game-filters-cli` 分支。
3. 要求 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并查看更改的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认它们准确说明了代码。

## 总结和后续步骤

你了解了 Copilot CLI 如何读取指令文件，并将新标准应用到功能代码中。具体来说，你：

- 了解了仓库指令和按路径应用的指令文件如何传递给代理。
- 更新了指令文件，确保遵循编码标准。
- 观察了指令文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot：指令文件及其他资源合集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
