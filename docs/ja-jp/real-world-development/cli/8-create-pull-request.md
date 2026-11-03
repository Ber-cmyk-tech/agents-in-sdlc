---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルター、指示、スキル更新、QA プロファイル、テストを確認し、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルターの実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストはすべて 1 つのブランチに保存されています。まとめてレビューし、プルリクエストを作成しましょう。星評価のプルリクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にマージの作業を任せます。

> [!NOTE]
> 通常は機能、指示の更新、スキル、QA エージェントを複数の PR に分けます。このワークショップでは手順を簡潔にするため、フィルターと品質ワークフローを 1 つの会話とブランチで進め、この PR にまとめました。

このレッスンでは、次の操作を行います。

- Agent Merge と、マージのライフサイクルを自動化する方法を学びます。
- 機能全体の変更と検証結果を確認します。
- フィルターの PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルターの開発では、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys では残りの PR 作業を自動化したいと考えていますが、マージを許可する権限は開発者の管理下に置きます。

## Agent Merge について

**Agent Merge** は、プルリクエストをマージするために必要な残りの作業を自動化します。有効にすると、Copilot は CI チェックの失敗やレビューコメントに対応し、必要に応じてリベースするなど、PR を妨げている問題に取り組みます。リポジトリで許可されている場合は、GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択しました。Agent Merge にその作業を任せることもできます。Agent Merge を有効にする前に、作業をレビューして準備ができていることを確認してください。

## Agent Merge で PR を管理する

コードを作成したら、まとめてレビューし、PR を作成して、残りの作業を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを確認します。すべて成功すると、マージが実行されます。
7. Copilot から「PR #14 was squash-merged successfully.」などのメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限不足を回避しません。処理がブロックされた場合は理由を確認し、リポジトリの設定で許可されていればレビュー済みの変更を手動でマージしてください。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストの処理など、複数の開発作業を自動化しました。このレッスンでは次の操作を行いました。

- Agent Merge と、マージのライフサイクルを自動化する方法を学びました。
- 機能全体の変更と検証結果を確認しました。
- フィルターの PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、クラウドへの委任などに使える[便利な Copilot CLI のスラッシュコマンドを確認します][next-lesson]。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
