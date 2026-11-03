---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の演習を始める前に、準備が必要です。Tailspin Toys リポジトリのコピーを作成し、作業用の [Codespace][codespaces] を起動して、エディターで GitHub Copilot Chat が使えることを確認します。

## ラボ リポジトリをセットアップする

コードの作業に使うリポジトリを作るため、[テンプレート][template-repository]から自分用のコピーを作成します。新しいリポジトリにはラボに必要なファイルがすべて含まれます。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]に移動します。
2. ラボ リポジトリ ページで **Use this template** ボタンを選択し、次に **Create a new repository** を選びます。

   ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft のイベントの一環としてワークショップに参加している場合は、メンターの指示に従います。それ以外の場合は、GitHub Copilot を利用できる組織にリポジトリを作成します。

   ![リポジトリ テンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) をメモします。後で使います。

> [!NOTE]
> **バックログは準備済みです**
>
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログも自動的に作成されます。ワークショップではこの Issue を使うため、自分で登録する必要はありません。

## Codespace を作成する

Codespace を使って演習を進めます。

[GitHub Codespaces][codespaces] は、ブラウザーでコードの記述、実行、デバッグを行うクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールをサポートする IDE を提供します。

1. 作成したリポジトリに移動します。
2. 緑色の **Code** ボタンを選択します。

   ![Code ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選び、**+** ボタンで新しい Codespace を作成します。

   ![新しい Codespace を作成する](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかります。サービスを手動でインストールするよりも短時間で済むため、待っている間に GitHub Copilot の機能を確認してもかまいません。

> [!CAUTION]
> 後の演習で Codespace に戻ります。今はブラウザーのタブを開いたままにしてください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers] で動作します。どちらも必要な前提条件が揃った環境を提供します。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、求められたら **Reopen in Container** を選択します。Codespace と同じ開発コンテナーが構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## GitHub Copilot Chat とエージェント モードを使う

GitHub Copilot Chat のエージェント モードを使うには、IDE に GitHub Copilot Chat 拡張機能が必要です。GitHub Codespace を利用している場合は、通常すでにインストールされています。

> [!TIP]
> GitHub Copilot Chat 拡張機能がインストールされていない場合は、[Visual Studio Code Marketplace からインストール][copilot-chat-extension]できます。または VS Code の拡張機能ビューを開き、**GitHub Copilot Chat** を検索して **Install** を選択します。

拡張機能をインストールしたら、GitHub アカウントでの認証が必要になる場合があります。

1. Codespace に戻ります。
2. エディターの右側に Copilot Chat が表示されていない場合は、Codespace の上部にある **Copilot Chat** アイコンを選択します。
3. Copilot Chat ウィンドウに「Hello world」などのメッセージを入力して Enter キーを押し、Copilot Chat を起動します。
4. 未認証の場合は GitHub アカウントへのサインインが求められます。指示に従って認証します。

   ![Copilot Chat の認証プロンプトの例](../../../_images/ex1-copilot-authentication.png)

5. 認証後、Copilot Chat ウィンドウが表示されます。

## まとめ

ラボ リポジトリのコピーを作成し、コードを書くときに使う Codespace の作成を開始しました。

## 次のステップ

Copilot を使って作業を始めましょう。次は[演習 1 - カスタム指示][next-lesson]に進み、Copilot にプロジェクトの慣例を伝えます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
