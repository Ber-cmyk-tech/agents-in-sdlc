---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

簡単なアプリケーションの変更でも、複数のファイルを更新し、テストを実行する必要があります。開発者は関連ファイルを探し、変更し、テストを実行して、見落としたファイルを調べ、さらに変更を加えることになります。

このような作業に役立つのが Copilot Agent Mode です。

Copilot Agent Mode は IDE 内でより自律的に作業します。開発者のように既存のプロジェクト構成を調べ、必要な更新を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。ここでは、Agent Mode を使ってサイトに機能を追加します。

> [!NOTE]
> 名前は似ていますが、エージェント モードと cloud agent は異なる用途向けです。エージェント モードは IDE 内で作業し、すばやいフィードバックと対話を行います。cloud agent はチームの一員のように非同期で作業し、Issue やプルリクエストを通じてやり取りします。

この演習では次のことを学びます。

- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを見つけて、複数ファイルを連携して変更する方法
- Agent Mode で UI とデータレイヤーにわたって機能を実装する方法
- 変更をコードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法

## シナリオ

ゲームの一覧が増えてきたため、カテゴリと発行元で絞り込めるようにします。前の演習では発行元ヘルパーを追加しました。ここでは Copilot Agent Mode で残りのデータレイヤー、UI、テスト作業を行います。

## Tailspin Toys Web サイトを実行する

変更を加える前に、Tailspin Toys Web サイトの現在の機能を確認します。これは開発者をテーマにしたボードゲームのクラウドファンディング サイトです。単一の Astro アプリで、ビルド時に静的 HTML としてページをレンダリングします。ページのフロントマターから Drizzle ORM を介してローカル SQLite データベースを直接参照します。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセス ヘルパーは `src/lib/` にあり、Astro の `<script>` と標準 DOM API でインタラクティブな機能を追加します。

### Web サイトを起動する

`npm run dev` タスクで Astro 開発サーバーを起動できます。

1. Codespace に戻ります。前の演習の後、`main` ブランチにいることを確認します。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. フィルター作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くダイアログが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルに表示された `http://localhost:4321` を選択します。

> [!NOTE]
> Codespace のターミナルから localhost のリンクを選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは Codespace 上の開発サーバーへのプライベート トンネルです。

### Web サイトを確認する

主な機能を確認します。

- **ホームページ**: タイトル、画像、説明を含むボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどの情報を表示します。

## Copilot でバックログを調べる

> [!TIP]
> **Copilot Chat を開く**
>
> Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順に記載します。

サイトは動作していますが、さらに機能を追加します。テンプレートからプロジェクトをセットアップした際に Issue のバックログが自動作成されています。Copilot に確認を依頼します。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選びます。Agent は複数ファイルを計画・実装し、ターミナル コマンドやツールを実行できます。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選びます。

> [!CAUTION]
> ワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップ作成時に Claude Sonnet 4.5 を使ったため、手順に含めています。LLM は確率的に動作するため、提案内容が異なる場合があります。これは通常の動作です。

> [!NOTE]
> Copilot は異なる MCP コマンドを使う場合がありますが、タスクを完了できるはずです。

3. 次のプロンプトを Copilot に送信します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Issue を一覧表示するコマンドを実行するため **Continue** を選択します。
5. 生成された Issue 一覧を確認します。

Copilot はユーザーに最も役立つと考える順に優先度を付けています。

## 指示ファイルを確認する

コード生成を開始する前に、Copilot にコンテキストを与える指示ファイルを確認します。Web サイトの機能追加方法を説明する [UI 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加の全体的な指針を確認します。
   - アーキテクチャの概要
   - コンポーネント設計、テスト容易性、アクセシビリティの原則
   - Astro、Tailwind CSS、Drizzle などのファイル種別に関する指示へのリンク

> [!TIP]
> 指示ファイルから、他の指示ファイルやプロジェクトのファイルを参照できます。パスは指示ファイルの場所からの相対パスです。指示を再利用したり、複雑な内容を小さなファイルに分割したり、Copilot に例を示したりできます。

## フィルター機能を実装する

フィルターの完成には、少なくとも 3 つの変更が必要です。

- データレイヤー (`src/lib/`) のフィルター ロジックを追加または調整する
- フィルター動作のテストを追加または更新する
- ゲーム一覧ページにフィルター UI を追加する

マージ前にテストを実行し、成功を確認する必要があります。Copilot Agent Mode にこれらの作業を依頼します。

1. 現在の Copilot 会話を続けるか、**New Chat** を選んで新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選んで指示ファイルを追加します。

   ![UI 指示ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. バックログの関連 Issue に基づいて実装するよう、次のプロンプトを送ります。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、データレイヤー、UI、テストの関連ファイルを見つけて変更し、テストを実行する様子を確認します。

   ![Copilot がプロジェクト ファイルを調べるスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの確認、ファイルの変更、テストなどを実行します。タスクやコードベースによって数分かかることがあります。作業中、コード エディターに **Keep** と **Undo** が表示される場合があります。作業中に選択する必要はなく、完了するとすべての変更に対する選択肢が表示されます。

7. Copilot に求められたら、テストを実行するため **Continue** を選びます。

   ![Copilot Chat でテスト実行を確認するダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に処理が一時停止したり、テストが失敗したりする場合があります。Copilot はエラーがないと判断するまでコード生成とテストを繰り返します。

   ![Copilot Agent Mode の完了した Chat セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成コードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新機能を試します。
11. すべて動作し、コードのレビューも済んだら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って Tailspin Toys に機能を追加する方法を確認しました。この演習では、次の操作を行いました。

- Agent Mode で UI とデータレイヤーにわたって機能を実装しました。
- プロジェクトを調べ、関連ファイルを特定し、複数ファイルを連携して変更しました。
- コードベースにマージする前に、変更とテストをレビューしました。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加の演習 — ページネーションを実装する

ゲーム一覧が増えるとページネーションが必要になります。この演習で学んだことを使い、Copilot にページネーションの実装を依頼してみましょう。

- 既存の指示ファイルを使うなど、現在のベスト プラクティスに従います。
- ユーザーがページ サイズを選べるようにするか、固定にするかを検討します。
- 希望どおりの実装になるよう、必要な背景情報をプロンプトに含めます。
- Copilot に変更や追加のコンテキストを依頼し、反復します。これは Copilot を使う通常の作業方法です。

## リソース

- [Copilot の Ask、Edit、Agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェント モード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
