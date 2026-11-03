---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩む組織は少なくありません。未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot cloud agent は、コードの更新や機能追加などを自律的に実行します。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を任せることで、開発者はより大きな課題に集中できます。

Copilot cloud agent では、次の内容を学びます。

- コード生成環境のカスタマイズ
- 操作を安全に行う方法
- 明確な範囲を定めた Issue の重要性
- Issue を Copilot に割り当てる方法

## シナリオ

Tailspin Toys には解消したい技術的負債があります。最初のサイトを作成した担当者が十分なドキュメントを残していません。まず、アプリケーションのすべてのエクスポート関数に TSDoc ドキュメントコメントを追加します。

また、デザイン チームはゲームの発見方法を改善したいと考えています。各ゲームの詳細ページに、同じカテゴリの他のタイトルを「関連ゲーム」として表示し、UX の受け入れテストに使える状態にします。この対応は現在のブロッカーですが、より優先度の高い Issue もあります。

このような作業は優先順位が下がりやすいものです。Copilot cloud agent に非同期で任せれば、開発者は別の作業を行い、後から結果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent][copilot-agents] は、人間の開発者と同様にバックグラウンドでタスクを実行します。たとえば、[GitHub Issue を Copilot に割り当て][assign-issue]ると、Copilot は進行状況を追跡するドラフト プルリクエストを作成し、環境を準備して、タスクに取りかかります。実行中でも完了後でもセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を適切に定めた指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。簡単そうに見える操作でも、詳しく調べると複雑なことがあります。[cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に定めてください。AI ペア プログラマーとして扱い、段階的に進め、学び、試し、改善します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習ではカスタム指示とその役割を学びました。すでに `.github/copilot-instructions.md` を使ったことがある場合は、ここで効果を簡単に確認しましょう。cloud agent に作業を割り当てる前に、このリポジトリに用意された指示ファイルを読み取り専用で確認します。

GitHub Web UI または起動済みの Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクション、特に TypeScript の慣例と TSDoc コメントに関する内容を確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` フロントマターが `**/*.test.ts` に指示を適用することを確認します。

次のセクションで **Code lacks documentation** Issue を cloud agent に割り当てると、プルリクエストに TSDoc コメント、TypeScript の慣例、コメント ヘッダーが反映されます。後の演習でも、PR のレビュー時に確認します。

## Copilot cloud agent の開発環境を準備する

コードを作成するときには、通常、必要なツールを用意し、環境を整えるセットアップが必要です。Copilot にタスクを割り当てる場合も同様で、cloud agent はソフトウェア エンジニアのように作業します。

cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に[セットアップ ワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係を準備できます。このラボでは、Node.js、プロジェクト依存関係、E2E テスト用ブラウザー、単一 Astro アプリ用にマイグレーションとシードを行った SQLite データベースを利用できるよう、事前に構成してあります。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *手順*に必要な最小限の権限を設定します。Copilot の操作には専用のトークンが付与されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順でリポジトリをクローンする場合は `contents: read` 権限が必要です。クローンしない場合、手順の完了後に Copilot が自動でクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL です)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを使えるように、ローカル SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同じ形式ですが、次の点に注意します。

- `copilot-setup-steps` という 1 つのジョブがあり、Copilot が PR 作業を開始する前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーがあるため、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot の実行を待つ前にワークフローをテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、多くのプロジェクトでは情報が古いか、まったくありません。この技術的負債は、開発の生産性を下げ、コードベースの保守や新しいメンバーの受け入れを難しくします。Copilot はドキュメントの作成に役立つため、この作業を cloud agent に割り当てます。後の演習で結果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して Issue 作成画面を開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当て画面を開きます。

   ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. `[WIP]: Code lacks documentation` のようなタイトルの PR を開きます。一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が To Do リストを作成したことを確認します。

> [!NOTE]
> Copilot が To Do リストを作成するまで数分かかることがあります。Copilot は環境を作成するために前述のワークフローを実行し、プロジェクトを分析して、作業方法を検討しています。

13. リストを確認し、実行予定のタスクを調べます。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新されたインジケーターを見るには、ウィンドウを更新する必要がある場合があります。

16. セッションをスクロールして Copilot の作業を確認します。コードを調べ、現状を把握し、計画を考え、コードを作成する過程が表示されます。

この作業には数分かかります。cloud agent の主な利点は非同期にタスクを進められることです。別の作業に取り組むため、もう 1 つのタスクも割り当ててからアプリへの機能追加を行います。

## ゲーム詳細ページに関連ゲームを追加する

cloud agent の利点の 1 つは作業を分担できることです。デザイン チーム向けに関連ゲームを追加する作業を Copilot に任せましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択し、**New issue** を選びます。
3. 空白の Issue テンプレートを選択します。
4. **Title** に `Show related games on the game details page` と入力します。
5. **Description** に次の内容を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

6. **Create** を選択し、右側の **Assign to Copilot** を開きます。

   ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

7. **Assign** を選択します。

まもなく、最初の Issue のコメントに 👀 が表示され、Copilot が作業中であることが分かります。

![Copilot が Issue で作業中であることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。cloud agent はソフトウェア エンジニアと同じように作業するので、完了後にレビューすればよく、常に監視する必要はありません。次はカスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を使い、Issue を割り当てて非同期に実行する方法を確認しました。技術的負債への対応、新機能の作成、フレームワーク間のコード移行などを Copilot に依頼できます。

この演習では、次の内容を学びました。

- コード生成環境のカスタマイズ
- 操作を安全に行う方法
- 明確な範囲を定めた Issue の重要性
- Issue を Copilot に割り当てる方法

cloud agent がバックグラウンドで作業している間に、次はカスタムエージェントを作成して使います。[Copilot cloud agent は MCP サーバー][cloud-agent-mcp]と、前のモジュールで設定したカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
