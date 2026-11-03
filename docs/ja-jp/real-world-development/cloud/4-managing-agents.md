---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の数回の演習では、Copilot cloud agent にユーザー エクスペリエンスの改善や機能追加に関する 3 つの別々のタスクを依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクを監視することも重要です。

GitHub.com の[エージェント ページ][agents-page]など、cloud agent に割り当てたタスクを管理するツールがあります。この Mission control では、オープンなプルリクエスト (PR) があるタスクを確認し、実行された操作を調べ、進行中のセッションに追加の指示を送れます。

このレッスンでは、次の操作を行います。

- エージェント ページで cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

ハイ コントラスト モードの作成をエージェントに依頼した後、チームはライト モードも追加するとよいと考えました。サイトのスタイルと切り替え機能をすでに更新しているため、ライト モードも含めるのが自然です。エージェントに追加の指示を送り、両方のモードを実装します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左ペインのタスク一覧を確認します。次のようなタスクが表示されます。
   - コードベースのドキュメントを更新する
   - ゲーム変更用の API を生成する
   - Web サイトにハイ コントラスト モードを追加する
3. 実行中のタスクを選択し、Copilot が行った操作を確認します。たとえば、次の操作が表示されます。
   - リポジトリのチェックアウト
   - Copilot の作業環境の作成
   - MCP サーバーのセットアップ
   - タスクを完了するための作業

> [!NOTE]
> 表示される手順は Copilot の作業状況や進め方によって異なります。

4. 右側に表示される PR ペインで、PR と変更されたファイルも確認します。

## cloud agent に追加の指示を送る

進行中のタスクを確認したので、ハイ コントラスト モードの作業中にライト モードも追加するよう Copilot に依頼します。

1. ハイ コントラスト モードのセッションを選択します。セッション名は Copilot が付けた名前や作業状況によって異なります。

   ![Mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが終わり、作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![エージェント ページで実行中の cloud agent に指示を送るダイアログ](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot が指示を受け取り、作業に加えたことを確認します。

## Copilot の作業を待つ

更新したタスクに対して Copilot が作業します。メッセージを送信した時点で実行中の手順が終わると、新しい依頼が作業の流れに追加されます。

この処理には数分かかります。待つ間に、ここまでに学んだことを振り返りましょう。

## まとめと次のステップ

Copilot agents ページを使って cloud agent のタスクを監視し、進行中の作業に追加の依頼を送る方法を学びました。この Mission control では、進行中や完了済みのタスクを追跡し、作業内容を確認して、スコープや指示を変更できます。

このレッスンでは次の内容を学びました。

- Mission control とエージェント ページで cloud agent のタスクを監視する方法
- 進行中のセッションに追加機能を依頼する方法

Copilot がアクセシビリティ機能を実装している間に、次は Copilot が作成した[プルリクエストを改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
