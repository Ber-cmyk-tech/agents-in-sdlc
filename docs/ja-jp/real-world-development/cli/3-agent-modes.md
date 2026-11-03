---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で実装方針を合意し、Autopilot で Issue に基づくフィルターを作成して、Interactive モードで結果を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初は小さな機能を追加しましたが、大きな変更にはより堅実なプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに沿って動作します。このレッスンから数回にわたり、Issue から新機能を作り、コードと動作を検証し、プロジェクトにマージする、一般的なエージェント駆動の開発プロセスを体験します。

> [!NOTE]
> 機能の作業を続ける間、同じ会話とブランチを使います。通常はファイルの種類ごとに異なるブランチや PR を使いますが、ここでは中核となる概念に集中するため簡略化します。

このレッスンでは、次の操作を行います。

- GitHub Issue から Copilot の会話を開始します。
- Plan モードで要件を定義します。
- Autopilot モードで機能を実装します。
- コードをレビューします。
- 転送ブラウザーで機能を手動で検証します。

この後のレッスンでは、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP で検証し、QA エージェントを作成して、機能 PR を作成します。

## シナリオ

Tailspin Toys のカタログが増えており、訪問者はカテゴリや発行元でゲームを絞り込む必要があります。バックログの Issue に機能の説明がありますが、カテゴリを組み合わせる方法など、コーディング前に合意すべき詳細があります。Plan モードで決定し、Autopilot に範囲を定めた実装を許可します。

## 背景

AI コーディングエージェントを開発フローに導入しても、基本は変わりません。むしろ、より重要になります。一般的な開発フローは次のとおりです。

1. 実装内容を記述した Issue から始めます。
2. 作成するものの計画を立てます。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> チームや組織によって詳細は異なりますが、多くの場合は上記の流れに似ています。

この標準的な手順に従うことで、AI が生成したコードが要件を満たし、手書きのコードと同じ確認プロセスを経るようにできます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> でモードを切り替えます。

- **Interactive**: エージェントと一緒に作業します。エージェントは変更を提案し、次に進む前に入力を待ちます。
- **Plan**: エージェントは最初に計画を作成し、プロジェクトファイルの編集は行いません。
- **Autopilot**: エージェントがコードの作成、テストの実行、タスク完了までの反復を自律的に行います。

最初に Plan モードで計画をレビューしてから、Autopilot で実装します。

## Issue から始める

フィルターの作業を始める前に、Codespace に戻り、リポジトリとターミナルを準備します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新してフィルター作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** という Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が追加されます。`#` を使うと、GitHub 上の Issue やプルリクエスト (PR) をコンテキストに含められます。

## フィルター機能を計画する

計画を立てることで、Copilot に任せる前に実装方針やタスクの進め方を決められます。複雑な作業では、まず時間をかけて計画することをお勧めします。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替え、プロンプトの下に **Plan** と表示されることを確認します。
2. 先ほど追加した Issue の後ろに、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、適切な方法を検討して計画を作成します。

3. フィルターの動作について質問された場合は、希望に基づいて回答します。唯一の正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) で計画を展開します。
5. 計画を上下にスクロールしてレビューします。
6. 決定と異なる部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたら、実装に進みます。Autopilot を使うと、完了と判断するまで Copilot がタスクに取り組みます。

1. **Accept plan and build on autopilot (recommended)** またはインストールしたバージョンで同等の選択肢を選びます。
2. プロンプトの下に **Autopilot** と表示されることを確認します。
3. Copilot が計画に沿ってコードを生成し、テストを実行する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装が始まる場合があります。先に計画をレビューしてください。依存関係の不足やポートの競合が報告された場合は、チェック完了とみなす前にセットアップの問題を解決します。

## 実装をレビューして検証する

コードが生成されたら、他のコードと同じくマージ前にレビューします。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替え、**Plan** や **Autopilot** の表示が消えたことを確認します。
2. `/diff` を実行して、フィルターの実装とテストを確認します。
3. 結果を Issue と計画時の決定に照らし合わせます。
4. レビューが終わったら <kbd>Esc</kbd> を押して差分ビューを閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば修正を依頼します。

## 新機能を試す

コードが正しく見えても、実際に動くか確認しましょう。前回と同じようにアプリを起動し、Codespaces の転送ポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. 計画で決めたカテゴリと発行元のフィルター、およびその組み合わせを試します。
4. リセットと結果なしの場合の動作が、Issue と決定内容に合っていることを確認します。
5. Codespace に戻り、Copilot に開発サーバーの停止を依頼します。

## まとめと次のステップ

異なる会話モードを使って機能を作成し、レビューしました。このレッスンでは次の操作を行いました。

- GitHub Issue から Copilot の会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送ブラウザーで機能を手動で検証しました。

次は、[カスタム指示を使って][next-lesson]コードが文書化された慣例に従うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
