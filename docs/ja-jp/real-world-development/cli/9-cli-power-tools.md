---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、任意のクラウドへの委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI には対話に使えるスラッシュコマンドが多数あります。高度な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、ほかの便利なコマンドをいくつか確認します。

このレッスンでは、次の操作を行います。

- `/context` と `/compact` で、Copilot が会話コンテキストを管理する方法を確認します。
- `/model` で利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認します。

## シナリオ

CLI の中核となるワークフローを終えました。次は、コンテキスト管理、モデル切り替え、セッション共有、[Copilot cloud agent][about-cloud-agent]への任意の委任などを見ていきます。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルのコンテキスト ウィンドウの上限に達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュコマンドを使って自分で状況を確認し、圧縮することもできます。

1. Codespace に戻り、Copilot CLI が開いていない場合はリポジトリ ルートで起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどのように配分されているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を実行して結果を比較します。会話が短い場合は、大きな違いがないこともあります。

> [!NOTE]
> コンテキスト ウィンドウがいっぱいになると、Copilot CLI は自動的に圧縮します。自分のタイミングで圧縮する場合は `/compact` を使います。関係のない別タスクに移るときに会話を新しくするには、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも異なります。Copilot CLI では、利用可能なモデルを一覧表示して選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況に関する情報を確認します。
3. 現在のモデルを維持するか、別のモデルを選ぶか、<kbd>Esc</kbd> で一覧を閉じます。

## セッションを共有する

チームで作業し、学んだことを共有すると、AI ツールの活用方法を改善できます。`/share` コマンドを使うと、Markdown または HTML ファイルへのエクスポート、共有リンクの作成、GitHub gist の公開ができます。

1. `/help` を入力し、インストール済みバージョンの `/share` オプションを確認します。
2. セッションを共有する場合は、ローカルの Markdown への出力に `/share file` を使うなど、目的に合った出力先を選びます。
3. エクスポート内容を他の人に送信したり公開したりする前に確認します。セッションにはプロンプト、応答、プロジェクトの詳細が含まれることがあります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容は公開しないでください。

## 任意: 作業の拡張や委任

ワークショップの中核となる作業は完了しました。大きなタスクには、Copilot CLI の次のコマンドも利用できます。

- `/fleet` は、独立したサブタスクを複数のサブエージェントに分割し、並列実行できます。
- `/worktree` は別のタスク用に独立した Git worktree を作成できます。
- `/delegate` は Copilot cloud agent にタスクを送信します。エージェントは非同期に作業し、プルリクエストを作成する場合があります。

これらのコマンドは追加の worktree やリモート作業を発生させることがあるため、任意の機能です。試すときは、範囲の明確な新しいタスクから始め、通常の手順で結果をレビューします。非同期エージェントについて詳しく知りたい場合は、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使うと、設定を変更したり、コンテキストを確認したりできます。このレッスンでは次の操作を行いました。

- `/context` と `/compact` でコンテキスト管理を確認しました。
- `/model` で利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並列作業、worktree、cloud agent への任意の委任を確認しました。

ほかにも多くのスラッシュコマンドがあります。学んだことを[振り返り][next-lesson]、さらに学ぶための次のステップを確認しましょう。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
