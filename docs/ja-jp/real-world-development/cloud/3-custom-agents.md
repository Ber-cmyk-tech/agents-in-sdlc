---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]は、開発ワークフローの特定のタスクや分野に合わせた AI アシスタントです。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成し、特定の作業を効果的に進めるための指示、ベスト プラクティス、コーディング パターン、専門知識を定義します。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディングを確認するセキュリティ エージェント、一貫したテスト パターンを適用するテスト エージェントを作れます。

カスタムエージェントは、プロジェクト内の `.github/agents` またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を記載する Markdown プロンプトが必要です。

### カスタムエージェントとエージェント スキルの違い

カスタムエージェントと[エージェント スキル][agent-skills-concept]には重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。分かりやすく言うと、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは独自のコンテキスト ウィンドウを持ち、スキル (場合によっては他のエージェント) を調整して作業します。このラボでは、アクセシビリティのカスタムエージェントがサイトをガイドラインに照らして確認し、更新します。その作業の中で、プルリクエスト ワークフローやテスト実行を支援するスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI の他の使い方と同様に、環境やシナリオに合わせて試し、改善してください。

この演習では次の内容を学びます。

- カスタムエージェントの定義方法
- タスクをカスタムエージェントに割り当てる方法

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを使えるようにしたいと考えています。現在のダークテーマは、テキストと背景色のコントラストが不十分で読みにくいというフィードバックがありました。デザイン チームは、ユーザーが切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なため、カスタムエージェントを使って機能をすばやく作成します。

## アクセシビリティ カスタムエージェントを確認する

アクセシビリティ向けのカスタムエージェントがすでに用意されています。内容を確認して、Copilot をどのように導くかを理解します。

Codespace に戻り、アクセシビリティ カスタムエージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを持つ YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには `name` と `description` を含むフロントマターが必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成に関する主要な責務
   - アクセシビリティのベスト プラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題や間違い

## Issue を作成して割り当てる

Mission control は、環境内のエージェントを操作する中心的な場所です。Copilot cloud agent にタスクを割り当て、進行中の作業を監視し、追加の指示を送れます。まず、ハイ コントラスト モードの作成を Copilot に依頼します。

1. リポジトリに移動します。
2. **Issues** タブを選びます。
3. **New issue** を選択し、空白の Issue を作成します。
4. **Title** に `Add high contrast mode to website` と入力します。
5. **Description** に次の内容を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

6. **Create** を選択して Issue を作成します。
7. 右側の **Assign to Copilot** を選択して割り当て画面を開きます。
8. 一覧から **Accessibility agent** を選びます。

   ![カスタムエージェントと Accessibility が選択された cloud agent の割り当て画面](../../../_images/ex5-select-custom-agent.png)

9. **Assign** を選択します。

Copilot はバックグラウンドで作業を始めます。

## まとめと次のステップ

特定のタスクや分野に合わせた GitHub Copilot の[カスタムエージェント][custom-agents]を確認しました。カスタムエージェントを使うと、チームの専門知識や標準を再利用できる形で定義し、Copilot の作業を導けます。

この演習では次の内容を学びました。

- カスタムエージェントの定義方法
- タスクをカスタムエージェントに割り当てる方法

Copilot がハイ コントラスト モードを実装している間に、次は Mission control で[エージェントのセッションを監視して方向を示します][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使う準備][org-custom-agents]
- [Enterprise でカスタムエージェントを使う準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
