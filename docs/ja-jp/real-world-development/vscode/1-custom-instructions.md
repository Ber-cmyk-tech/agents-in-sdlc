---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を使う際はコンテキストが重要です。タスクを特定の方法で実行したい場合や、Copilot に背景情報を知っておいてほしい場合は、そのコンテキストを利用できるようにします。Copilot にコード生成の指針を伝える主な方法の 1 つが[指示ファイル][instruction-files]です。

この演習では次の操作を行います。

- リポジトリのカスタム指示とパス別の指示ファイルを使い、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- 現在の指示のまま、フィルター用の最初のデータ部分 (発行元ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再度実行し、生成コードが新しい標準に従うことを確認します。
- 指示の変更とフィルターの一部を `main` にコミットし、次の演習で更新後の指針を使えるようにします。

> [!CAUTION]
> 生成コードが、指定した標準から外れる場合があります。Copilot の動作は確定的ではありません。この演習では、出力を完全に同じにするのではなく、指示の更新後に動作がどのように変わるかを確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には次の開発ガイドラインがあります。

- データレイヤーには必ずユニットテストを用意します。
- UI はダークモードで、現代的な見た目にします。
- TSDoc のドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントを記載します。

指示ファイルを使って、Copilot がこの方針に沿って作業できるようにします。

### カスタム指示

カスタム指示は、コーディングスタイルや要件などのコンテキストを Copilot に提供し、より適切な提案やコード生成を促します。リポジトリ全体、または特定のファイル種別やタスクに指示を指定できます。

- `.github/copilot-instructions.md` は、リポジトリのすべてのリクエストで Copilot に渡す単一の指示ファイルです。技術スタック、プロジェクトの概要、ベスト プラクティスなど、ほとんどのチャットや CLI リクエストに関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別に作成します。TypeScript や Astro、UI コンポーネント、ユニットテストなどの指針を指定できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示と、`applyTo` フロントマターを持つ `*.instructions.md` ファイルをコード生成に使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準の指示ファイルにも対応しています。[カスタム指示のサポート][custom-instructions-support]を参照してください。

### 指示ファイルを管理するベスト プラクティス

サンプル プロジェクトには、実用的な指示ファイルの構成例があります。基本的には、次の方法をお勧めします。

- `copilot-instructions.md` には、作成中のもの、プロジェクト構成、全体に適用するコーディング標準など、プロジェクト レベルの指針を記載します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル種別またはタスク固有の指示を記載します。
- 自然な言葉で、明確なガイドラインを記載します。コードの良い例と避けるべき例を示します。

指示ファイルの作成方法は 1 つではありません。プロジェクトで試しながら、最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルを用意することをお勧めします。プロジェクトには [UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に指示ファイルを作成させることもできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` など、利用する環境によって方法が異なります。
>
> テンプレートを探している場合は、指示ファイル、カスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに用意された `copilot-instructions.md` と `*.instructions.md` ファイルをエディターまたは GitHub Web UI で確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** にある **GitHub Actions Workflows** の指針も確認してください。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle データレイヤー、テストなどの指示ファイルを確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。`applyTo` フィールドがリポジトリ ルートからの glob を定義しており、たとえば `**/*.test.ts` に一致する TypeScript テストに適用されます。
5. このプロジェクトでユニットテストを作成するときの指示を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾にある他の指示ファイルや既存ファイルへのリンクを確認します。パスは指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次に TSDoc コメントとファイル コメントを追加します。

## 変更用ブランチを作成する

まず、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) で新しいターミナルを開きます。
3. ブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以降の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順に記載します。

カスタム指示の効果を確認するため、まず現在の指示ファイルのままでプロンプトを送信します。後で指示を更新し、同じプロンプトをもう一度送信して違いを見ます。

1. 前の演習から残っているエディター タブを閉じ、Copilot が必要なコンテキストだけを使うようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを作成する場所を示します。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選び、ファイルの変更を適用できるようにします。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを確認してコードを更新します。通常、ヘルパー ファイルとテストが変更されます。
6. 提案されたヘルパーが `db` クライアントを最初の引数に取り、型付きの発行元配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規則に基づいています。
7. 提案されたコードに TSDoc コメントとファイル先頭のコメントが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメント コメントを追加する場合があります。その場合も問題ありません。指示を更新した後の一貫性向上を確認するのが目的です。

## リポジトリ標準を追加する

`.github/copilot-instructions.md` にプロジェクト全体の指針を追加します。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。現時点ではインライン ドキュメントの規則がありません。
3. 既存の標準の直後に、次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> リポジトリ全体の指示には `.github/copilot-instructions.md`、言語やファイル種別、タスクに固有の指示には `*.instructions.md` を使います。TSDoc コメントのようなプロジェクト全体の標準は、リポジトリ レベルの指示に適しています。

## 同じプロンプトを再送信して違いを確認する

1. Copilot Chat に戻り、**New Chat** を選択して新しい会話を開始します。
2. `src/lib/publishers.ts` を選び直し、Copilot が対象ファイルを把握できるようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 今度は提案されたファイルの先頭に、次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 関数に次のような TSDoc コメントが追加されていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しい標準を Copilot に伝え、次の演習の土台となる実際のコードに適用しました。

## 指示とブランチをコミットしてプッシュする

指示ファイルも他のリポジトリ資産と同じようにソース管理します。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) で新しいターミナルを開きます。
2. ターミナルで変更を確認します。

   ```bash
   git status
   ```

3. 指示の更新とヘルパーの変更をステージしてコミットします。テストなどの追加変更が `git status` に表示される場合は、それらも追加します。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

プッシュしたブランチから PR を作成し、バックログにあるドキュメント標準の Issue と関連付けます。Copilot に関連 Issue を探し、PR を作成してマージするよう依頼します。

> [!NOTE]
> Copilot に GitHub 上の操作を依頼するのはこれが初めてです。プロジェクト テンプレートでは GitHub ツールが構成済みです。Copilot が初めて利用するときに VS Code から GitHub サインインを求められた場合は、指示に従って認証します。この接続の仕組み (Model Context Protocol) は後の演習で説明します。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) で Copilot Chat ビューを開き、エージェント選択で **Agent** が選ばれていることを確認します。
3. 現在のブランチから PR を作成し、指示の更新と発行元ヘルパーの基礎作成の両方を説明するよう Copilot に依頼します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成します。
5. Copilot による GitHub 操作の **Allow** を求められた場合は、実行内容を確認して適切に許可します。
6. PR が作成されたら、次のプロンプトを使って PR をマージし、ローカル ブランチを `main` に戻すよう依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub 操作やシェル コマンドの **Allow** を求められた場合は、内容を確認して適切に許可します。

GitHub Copilot の支援で PR を作成し、マージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを得る方法を確認し、VS Code の Copilot Chat で次の作業を行いました。

- 既存の指示を使ってコード生成プロンプトを送り、提案を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再送信し、生成コードが新しい標準を反映することを確認しました。
- 指示の更新とヘルパーの土台を `main` にコミットし、次の演習で使えるようにしました。

次は[エージェント モード][next-lesson]を使い、コードベース全体にわたって機能を追加します。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
