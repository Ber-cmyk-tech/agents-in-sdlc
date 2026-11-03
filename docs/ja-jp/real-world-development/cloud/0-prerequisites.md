---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、準備が必要です。Tailspin Toys リポジトリのコピーを作成し、[Codespace][codespaces] を起動します。Codespace では指示ファイルを編集し、cloud agent が作成した内容をレビューします。

## ラボ リポジトリをセットアップする

コードの作業に使うリポジトリを作るため、[テンプレート][template-repository]から自分用のコピーを作成します。新しいリポジトリにはラボに必要なファイルがすべて含まれます。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]に移動します。
2. ラボ リポジトリ ページで **Use this template** を選択し、次に **Create a new repository** を選びます。

   ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft のイベントの一環としてワークショップに参加している場合は、メンターの指示に従います。それ以外の場合は、GitHub Copilot を利用できる組織にリポジトリを作成します。

   ![リポジトリ テンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) をメモします。ワークショップで後から使います。

## Codespace を作成する

Codespace を使ってラボの演習を進めます。

[GitHub Codespaces][codespaces] は、ブラウザーでコードの記述、実行、デバッグを行うクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールをサポートする IDE を提供します。

1. 作成したリポジトリに移動します。
2. 緑色の **Code** ボタンを選択します。

   ![Code ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選び、**+** ボタンで新しい Codespace を作成します。

   ![新しい Codespace を作成する](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、サービスを手動でインストールするよりも短時間で済みます。作成を待つ間に、次に紹介する GitHub Copilot の機能を確認してもかまいません。

> [!CAUTION]
> 後の演習で Codespace に戻ります。今はブラウザーのタブを開いたままにしてください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers] で動作します。どちらも必要な前提条件が揃った環境を提供します。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、求められたら **Reopen in Container** を選択します。Codespace と同じ開発コンテナーが構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

ラボ リポジトリのコピーを作成しました。また、Copilot cloud agent と一緒に作業するときに使う Codespace の作成を開始しました。

## 次のステップ

次は cloud agent が従うカスタム指示を追加します。[演習 1 - カスタム指示][next-lesson]に進んでください。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
