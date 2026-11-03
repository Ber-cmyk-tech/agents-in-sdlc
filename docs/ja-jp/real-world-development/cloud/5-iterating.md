---
title: "演習 5: GitHub Copilot の作業を反復する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスの改善や機能追加に関する複数のタスクを GitHub Copilot に依頼しました。コードにドキュメントを追加し、デザイン チームが改善を重ねられる関連ゲーム機能を作成し、ハイ コントラストとライト モードを含むアクセシビリティ機能を実装しました。コードの変更を確認し、必要に応じて Copilot に改善を依頼します。

### シナリオ

これまでに何度も説明したように、生成 AI を利用してもソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに従います。チーム全体にレビューを依頼する前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot の提案を確認しましょう。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期に、監督を受けずに作業するため、安全性を保つための制約が設けられています。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みは作業用ブランチに限定されます。
- cloud agent は GitHub Actions 内に一時的な専用環境を作成して実行されます。
- GitHub Actions ワークフローの実行には人間の承認が必要です。
- [外部リソースへのアクセスは既定で制限][agent-firewall]されています。MCP サーバーへのアクセスも制限されます。

## 生成されたドキュメントをレビューする

最初に、GitHub Copilot cloud agent が作成した、コードへのドキュメント追加に関するプルリクエスト (PR) を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR に GitHub Copilot がファイアウォールでブロックされているという警告が表示される場合があります。Copilot は MCP サーバーなどの外部リソースへのアクセスが既定で制限されているため、これは想定された動作です。必要であれば、[cloud agent のファイアウォールを構成または無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルの PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が付きます。その場合は完了まで待ちます。数分かかることがあります。

4. 準備ができたら **Files changed** タブを選び、変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc ドキュメントコメントや追加されたドキュメントを確認します。変更内容は状況によって異なります。演習 2 の開始時に確認したカスタム指示に基づく、TSDoc、TypeScript の慣例、コメント ヘッダーを確認してください。
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを示すインジケーターを探します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローが登録されます。単一 Astro アプリのプロジェクト チェックがすべて成功することを確認します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

Copilot との PR 作業は一方向ではありません。チーム メンバーに依頼するように、PR やコードのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、変更に対応する新しいセッションを開始します。結果は確定的でないため、必ずしも同じ文言を使う必要はありません。次のような変更を依頼できます。

- 各コード ファイルの先頭に、役割を簡潔に説明するコメント ヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc ドキュメントコメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 上記の例などを使い、生成されたドキュメントの変更を **@copilot** に依頼するコメントを追加します。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot は変更用に新しいセッションを開始します。
3. **Back to pull request** を選択すると PR に戻れます。

   ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、新しいコミットが PR に追加されます。
5. **Files changed** タブで変更をレビューします。

必要に応じて反復し、満足できたらドラフト PR をレビュー可能な状態にして `main` ブランチにマージします。

![PR をレビュー可能な状態にする](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対し、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を開きます。
4. **Files changed** タブで生成されたコードを確認します。
5. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローが承認待ちになっていることを示すインジケーターを確認します。
7. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. PR のチェック セクションにワークフローが登録されます。単一 Astro アプリのチェックがすべて成功することを確認します。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて、ターミナルから次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot は関連ゲーム機能を実装しました。前の PR と同様に、コメントで変更を依頼できます。たとえば表示する関連ゲームの数を調整したり、この Issue をカスタム指示の更新前に割り当てたことを踏まえて、コメント ヘッダーと TSDoc を追加したりできます。**Conversation** タブでコメントすると、Copilot が新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ エージェントで実装した機能を確認します。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 で追加を依頼したライト モードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合は、タイトルに **[WIP]** が付きます。完了まで待ちます。数分かかる場合があります。

4. **Files changed** タブで生成されたコードを確認します。
5. 実装をレビューし、特に次の点を確認します。
   - モード切り替え用の UI コンポーネント
   - ユーザー設定を保存する local storage の使用
   - ハイ コントラストとライト モードの CSS またはスタイル変更
   - ARIA ラベルやキーボード操作などのアクセシビリティ属性
   - モードを切り替える JavaScript/TypeScript コード
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを確認します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローが登録されます。単一 Astro アプリのチェックがすべて成功することを確認します。
10. **任意:** Codespace でブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Copilot が作成したブランチ名に置き換えて、ターミナルで次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   その後、アプリケーションを起動し、ハイ コントラストとライト モードをブラウザーで切り替え、ページを再読み込みしても設定が保持されることを確認します。

カスタム アクセシビリティ エージェントは、アクセシビリティのベスト プラクティスに従って機能を実装するよう Copilot を導きました。懸念点や改善点があれば、前の PR と同様にコメントで **@copilot** に変更を依頼できます。

## 任意の演習 — さらにタスクを委任する

cloud agent は、実際のバックログのタスクを任せて別の作業に取り組むときに役立ちます。習慣として身につけるため、リポジトリに Issue を追加して Copilot に割り当ててみてください。

- ゲーム詳細ページに支援者向け関心フォームを作る
- ゲーム一覧ページにページネーションを実装する
- データアクセス ヘルパーに入力検証とエラー処理を追加する

## まとめ

Cloud agent ハーネスを完了しました。このレッスンでは、次の操作を行いました。

- **リポジトリに用意されたカスタム指示ファイルを確認**し、cloud agent の出力に与える影響を確認しました。
- **Issue を Copilot cloud agent に割り当て**、環境準備、計画、非同期の作業を見ました。
- **カスタムエージェントを作成して利用**し、ハイ コントラストとライト モードを追加しました。
- **Copilot agents ページを Mission control として使い**、作業中のセッションを監視して方向を示しました。
- **cloud agent の PR をレビューして反復**し、`@copilot` に変更を依頼して、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。他のハーネスでは、ここで学んだことを別の環境で実践できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat のエージェントモードと MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェント スキル、QA カスタムエージェント、Playwright MCP、Agent Merge、任意の `/delegate` を使い、ターミナルから 2 つの変更をレビューしてリリースします。

自分のリポジトリでは、次のアイデアを試してください。

- リファクタリングやテスト範囲の Issue を cloud agent に割り当てます。
- 別の分野向けのカスタムエージェントを作成します。
- 別の技術スタック向けに `copilot-setup-steps.yml` を構成します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
