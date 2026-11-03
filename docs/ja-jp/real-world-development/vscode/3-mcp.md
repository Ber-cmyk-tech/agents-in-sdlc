---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェント モード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェント モードでフィルター機能を作成しました。プルリクエストを作成する前に、ブラウザーで実際に動くか確認しましょう。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続して、Copilot に実際のブラウザーから機能をテストしてもらいます。その後、ブランチを公開して PR を作成します。

この演習では次の操作を行います。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- ブラウザーでフィルター機能を手動テストするよう Copilot に依頼します。
- ブランチを公開し、フィルター作業の PR を作成します。

## Model Context Protocol (MCP) とは

エージェント モードはエディターの外にアクセスできると、さらに便利になります。Model Context Protocol (MCP) は Copilot が外部ツールやサービスと通信するための標準的な方法です。

![エージェント モードがコンテキスト、LLM、ツール (MCP サーバーや VS Code 拡張機能が提供するツールを含む) とやり取りする仕組みを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部のツールやサービスとリアルタイムに通信できます。リソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わる操作を行ったりできます。

ツールやリソースには、AI エージェントと外部サービス (既存の API や NPM パッケージなどのローカル ツール) を仲介する MCP サーバーを通じてアクセスします。各 MCP サーバーが、エージェントに利用可能なツールとリソースを提供します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスし、新しいリポジトリの作成、既存リポジトリの更新、Issue やプルリクエストの管理などを行います。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化を提供し、Web ページの移動、フォーム入力、ボタン選択などを行います。

さまざまなツールやリソースにアクセスする MCP サーバーがあります。GitHub は、見つけやすさとエコシステムへの貢献を促進するために [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーは他の依存関係と同様に扱ってください。利用前にソースコードを確認し、公開元を検証し、セキュリティへの影響を検討します。信頼できる MCP サーバーだけを使い、機密リソースや操作へのアクセス許可は慎重に判断してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーが構成済みであることを確認します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

この `github` エントリはプロジェクト テンプレートに含まれています。前の演習で Copilot が Issue のバックログを参照できたのはこのためです。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルへのインストールは不要です。VS Code は HTTP で接続し、Copilot がツールを初めて使うときに GitHub 認証を行います。

## Playwright MCP サーバーを追加する

[Playwright MCP サーバー][playwright-mcp-server]を追加すると、Copilot がブラウザーを操作して機能をテストできます。

1. `.vscode/mcp.json` の `github` と並べて `playwright` エントリを追加し、次のようにします。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. ファイルを保存します。

リモートの `github` サーバーと異なり、`playwright` はローカル サーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` は画面を表示せずにブラウザーを実行します。デスクトップ環境のない Codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys では E2E テストに Playwright を使うため、必要なブラウザーはすでにインストールされていることが多いです。Copilot がブラウザー不足を報告した場合は、`npx playwright install chromium` を実行するよう依頼します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて起動**します。Copilot が初めてサーバーのツールを使うとき、VS Code がサーバーを開始し、信頼するかどうかを確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に表示される **Start** 操作を選択し、サーバーを起動して構成を確認します。
2. サーバーを信頼する確認が表示されたら、構成をレビューし、信頼して起動することを選択します。

手動で起動する必要はありません。省略した場合は、次の手順で Copilot がツールを必要としたときに VS Code が起動し、信頼の確認を表示します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot がページを開き、移動し、フィルターを適用し、結果を読み取って報告できます。会話を離れずに機能を確認できます。

Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティ ツリー][playwright-mcp-server]を使って動作します。エージェントは支援技術と同様に、ラベル付きのボタン、リンク、リスト項目などの構造化要素を扱うため、簡単な機能確認がアクセシビリティの確認にもなります。

サーバーを接続し、アプリが実行中であることを確認してから、次のプロンプトを Copilot に送信します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーからブラウザーを起動し、各手順を実行して結果を報告します。Issue の受け入れ基準と照らし合わせます。問題があれば、PR を作成する前に追加の質問をするか、コードの修正を依頼します。

> [!NOTE]
> テストするにはアプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動します。Playwright MCP サーバーを初めて使うときにブラウザーのダウンロードが必要な場合があります。ブラウザー不足が報告されたら、`npx playwright install chromium` を実行して再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動くことを確認したので、チームでレビューするために PR を作成します。最初に `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンでコミット メッセージを生成します。

   ![Copilot の変更を示す Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub CLI など複数の方法で PR を作成できます。ここでは Copilot に関連する Issue を探し、現在のブランチから PR を作成して関連付けてもらいます。

1. Copilot Chat パネルに移動し、**New Chat** を選択します。
2. GitHub ツールを使えるように、エージェントの一覧で **Agent** が選択されていることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、Copilot が情報取得や操作を行えるようにします。GitHub ツールを初めて使うときに認証を求められた場合は、指示に従います。
5. Copilot が Issue を探して PR を作成したことを確認します。
6. Copilot が表示したリンクを選択して PR をレビューします。ただし、まだマージしないでください。

## まとめと次のステップ

この演習では次の操作を行いました。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する仕組みを学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- ブラウザーでフィルター機能を手動テストしました。
- ブランチを公開し、フィルター作業の PR を作成しました。

MCP サーバーで機能をテストしましたが、MCP は Copilot を拡張する方法の 1 つです。次は、アクセシビリティ レビューなどの作業を効率化する[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
