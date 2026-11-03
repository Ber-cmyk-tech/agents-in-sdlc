---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルターのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使う際はコンテキストが重要です。特定の方法でタスクを実行したい場合、その指針を Copilot が利用できるようにします。[指示ファイル][instruction-files]には、求めるコードだけでなく、その構成方法も記述できます。フィルター機能を実装したので、Copilot が使った指示を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次の操作を行います。

- リポジトリの指示ファイルとパス別の指示ファイルがエージェントに適用される方法を確認します。
- コーディング標準を守るため、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には開発作業のガイドラインがあります。

- コメントでは、コードの内容を繰り返すのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値、および該当する場合は注入可能な `db` 引数を記述する TSDoc/JSDoc を付けます。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明し、関連コードが変更されたらコメントを最新に保ちます。
- 既存のフォーマットと lint の指針を維持します。

指示ファイルを使って、Copilot がチームの方針に沿ってタスクを実行できるようにします。

## 指示ファイル

カスタム指示は、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコード生成を促します。リポジトリ全体、または特定のファイル種別に指示を指定できます。

- `.github/copilot-instructions.md` は、リポジトリ内のすべてのリクエストで Copilot に渡される単一の指示ファイルです。ほとんどのリクエストに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、言語、ファイル種別、タスク別のガイドラインを記載します。

> [!NOTE]
> その他の指示形式やサポート状況は環境によって異なります。特定の形式を利用する前に、[カスタム指示のサポート リファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには、変更前の影響を確認できる指示ファイルが用意されています。

1. Codespace に戻ります。
2. Codespaces エディターで `.github/copilot-instructions.md` を開きます (ターミナルではありません)。
3. プロジェクトの概要とコーディング指針を確認します。この指示は、このリポジトリでの Copilot とのすべての対話に適用されます。
4. `.github/instructions` フォルダーを開き、Astro、Drizzle データレイヤー、テストなどの指示ファイルを確認します。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象のファイルを示す glob です。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルや既存プロジェクト ファイルへのリンクを確認します。大きな指示セットを再利用可能なファイルに分け、Copilot が参考にできる例を示せます。

## チームの方針に合わせて指示ファイルを更新する

既存の指示に、生成される TypeScript に TSDoc コメントを追加する要件を加えます。

1. `.github/copilot-instructions.md` の **Code formatting guidance** セクションを探します。およそ 35 行目にあります。
2. 最後の箇条書きとして、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新した指針を使う

Copilot CLI は会話開始時にリポジトリの指示を読み込みます。更新した指示を読み込ませるには、終了してフィルターの会話を再開します。

1. フィルターの会話が開いている場合は `/exit` を入力します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリ ルートで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   求められた場合はフィルターの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新した指針を適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。次は、[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストを一貫して実行します。

## リソース

- [Copilot CLI のカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [効果的なカスタム指示の作成に関するベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
