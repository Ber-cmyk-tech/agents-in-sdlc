---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連のワークフローで GitHub Copilot CLI を使いました。次の作業を行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、用意されたフィルターの Issue を見つけました。
- 星評価を追加し、転送ブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルターの Issue から作業を始め、Plan モードで方針を定め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルターを確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選び、要件、テスト範囲、スキルの結果、ブラウザーでの検証結果を確認しました。
- フィルターの変更をまとめてレビューし、Agent Merge に PR の処理を許可しました。
- コンテキスト、モデル、共有、任意のクラウドへの委任に使うスラッシュコマンドを確認しました。

## 実装したもの

このワークショップには、更新した `main` からそれぞれ作成したブランチ上で、2 つの PR があります。

1. **星評価**: ゲームカードに既存の `starRating` と未評価状態を明示して表示します。
2. **フィルターと品質ワークフロー**: フィルターを実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルと関連テストを追加します。

フィルターの計画から PR 作成までは同じ会話とブランチを使いました。ワークショップを簡潔にするため、作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を通じた Copilot のブラウザー確認など、複数の方法でコードを検証しました。`quality-checks` スキルは新しいレポート形式でユニットテスト、lint、型チェックを実行しました。QA はこれらの結果と要件およびテスト範囲のレビューをまとめ、PR の前に報告しました。

追加するテストは実際の不足を補うものであるべきです。QA の結果、新しいテストが不要と判断される場合もあります。マージを許可する前に、コードと根拠を確認し、変更後は関連する検証結果を更新してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業内容を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整できます。指示は標準を定め、スキルは繰り返し実行する作業を記述し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。構築前に方針を検討するときは **Plan**、範囲の明確な変更を対話しながら進めるときは **Interactive**、明確に範囲が定まったタスクを任せるときは **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

ツールや環境と同様に、コンテキストも重要です。何を、なぜ、どのように作るかを明確に伝えることで、出力は大きく変わります。

## さらに試す

中核となるワークフローは完了しました。ほかにも便利な CLI 機能があります。

- `/review` でコードレビューエージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を説明し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並列に調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` でタスクを Copilot cloud agent に送信します。

## 次のステップ

ツールを上達させるには、使い続けるのが一番です。製品コード、趣味のコード、長年作ろうと思っていた小さなアプリなどで活用してください。チームと学んだことを共有し、互いに学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のカスタマイズ][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
