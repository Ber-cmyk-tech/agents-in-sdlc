---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]は、開発ワークフローの特定のタスクや分野に合わせた AI アシスタントです。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成し、特定の作業を効果的に進めるための指示、ベスト プラクティス、コーディング パターン、専門知識を定義します。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディングを確認するセキュリティ エージェント、一貫したテスト パターンを適用するテスト エージェントを作れます。

カスタムエージェントは、プロジェクト内の `.github/agents` またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を記載する Markdown プロンプトが必要です。

### カスタムエージェントとエージェント スキルの違い

カスタムエージェントと[エージェント スキル][agent-skills-concept]には重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。分かりやすく言うと、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは独自のコンテキスト ウィンドウを持ち、スキルや他のエージェントを調整して作業します。このラボでは、アクセシビリティ エージェントがサイトをガイドラインに照らして確認・更新します。その作業の中で、プルリクエスト ワークフローやテスト実行を支援するスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI の他の使い方と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを使えるようにしたいと考えています。現在のダークテーマは、テキストと背景色のコントラストが不十分で読みにくいというフィードバックがありました。デザイン チームは、ユーザーが切り替えられるハイ コントラスト モードを求めています。

アクセシビリティ向けのカスタムエージェントを使って機能を実装します。この演習では次の操作を行います。

- 既存のアクセシビリティ カスタムエージェントを確認します。
- Copilot Chat のアクセシビリティ エージェントを使ってハイ コントラスト モードを実装します。

## アクセシビリティ カスタムエージェントを確認する

アクセシビリティ向けのカスタムエージェントがすでに用意されています。Codespace に戻り、アクセシビリティ作業用の `main` から新しいブランチを作成します。演習 3 のフィルター PR とは分けて作業します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを持つ YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには `name` と `description` を含むフロントマターが必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成に関する主要な責務
   - アクセシビリティのベスト プラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題や間違い

## Copilot Chat でカスタムエージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェント ドロップダウンには、`.github/agents` で定義されたカスタムエージェントが表示されます。エージェントを選ぶと、その指示とツールに合わせてチャット セッションが構成されます。

> [!TIP]
> **Copilot Chat を開く**
>
> Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順に記載します。

1. Chat ビューのエージェント一覧で **Agent** を選択します (まだ選択されていない場合)。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部のエージェント ドロップダウンを開きます。既定では **default** が選択されています。
3. 一覧から **Accessibility agent** を選びます。
4. 次のプロンプトをアクセシビリティ エージェントに送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat がコードベースを調べ、変更を提案して適用します。編集はファイル パスと差分とともにチャット内に表示されます。
6. エージェントが変更した作業ファイルが **Files changed** の表示に反映されます。

> [!NOTE]
> この作業には数分かかることがあります。Copilot は Astro コンポーネント、CSS、関連テストなどのファイルを実際に変更しています。

次の演習では、作業中の変更を確認し、指示を追加します。

## まとめと次のステップ

GitHub Copilot のカスタムエージェントを使って、特定のタスクや分野に合わせた専門的な役割を定義できることを確認しました。チームの専門知識や標準を再利用できる形で定義し、Copilot の作業を導けます。

このレッスンでは、次のことを学びました。

- カスタムエージェントの定義方法
- Copilot Chat のエージェント モードでカスタムエージェントを使う方法

次は[エージェントの作業を監視して方向を示します][next-lesson]。作業中の変更を確認し、同じセッションにライト モードを追加するよう依頼します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
