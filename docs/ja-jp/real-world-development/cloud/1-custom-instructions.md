---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使う際はコンテキストが重要です。タスクを特定の方法で実行したい場合や、Copilot に背景情報を知っておいてほしい場合は、そのコンテキストを利用できるようにします。[指示ファイル][instruction-files]を使うと、何を実行するかだけでなく、どのように実行するかも Copilot に伝えられます。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパス別の指示ファイルを使い、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスと異なり、この演習ではプロンプトの変更前後を比較しません。Copilot cloud agent は GitHub Issue に対して非同期で動作するため、リアルタイムの比較が難しいためです。このハーネスの後半で、指示ファイルが cloud agent のプルリクエストに与えた影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、次の開発ガイドラインがあります。

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

このリポジトリに含まれる `copilot-instructions.md` と `*.instructions.md` ファイルを、エディターまたは GitHub の Web UI で確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** にある **GitHub Actions Workflows** の指針も確認してください。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle データレイヤー、テストなどの指示ファイルを確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。`applyTo` フィールドがリポジトリ ルートからの glob を定義しており、たとえば `**/*.test.ts` に一致する TypeScript テストに適用されます。
5. このプロジェクトでユニットテストを作成するときの指示を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾にある他の指示ファイルや既存ファイルへのリンクを確認します。パスは指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次に TSDoc コメントとファイル コメントを追加します。

## リポジトリ標準を追加する

ここでは、TSDoc ドキュメントコメントとファイル ヘッダーをコード内に記載する標準を追加します。後の演習で Issue を割り当てるとき、cloud agent がこの指示を読み取ります。

cloud agent は Issue の対象ブランチから作業を開始します。次の演習で `main` からブランチを作成するため、指示を `main` にマージしておきます。

1. Codespace のターミナルで新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. `.github/copilot-instructions.md` を開きます。
3. 27 行目付近にある **Code formatting requirements** セクションを探します。現在はコード内のドキュメントを求める規則がありません。
4. 既存の標準の直後に、次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. `copilot-instructions.md` を保存します。

> [!TIP]
> リポジトリ全体の指示には `.github/copilot-instructions.md`、言語やファイル種別、タスクに固有の指示には `*.instructions.md` を使います。TSDoc コメントのようなプロジェクト全体の標準は、リポジトリ レベルの指示に適しています。

## 指示の更新をコミット、プッシュ、マージする

cloud agent は Issue の対象ブランチにある指示を読み込みます。次の演習で `main` に対して作業するため、この変更を `main` にマージします。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成してマージします。次の演習で Issue を割り当てると、cloud agent は更新された指示を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチ作成を省略して `main` に直接コミットしてもかまいません。ここでは実際のプロジェクトと同じ手順を練習するため、ブランチを使っています。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、リポジトリ全体の標準を `.github/copilot-instructions.md` に追加しました。残りの演習では、cloud agent が作成するプルリクエストで指示の影響を確認します。

次は、[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
