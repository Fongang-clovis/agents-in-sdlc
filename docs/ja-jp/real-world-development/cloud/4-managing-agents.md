---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

これまでの演習では、ユーザー体験の改善と機能追加を目的に、GitHub Copilot cloud agent に3つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視できることも重要です。

cloud agent に割り当てたタスクを管理する方法はいくつかあります。GitHub.com の[エージェントページ][agents-page]では、オープンなプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。作業内容を調べ、進行中のセッションに追加の指示を与えることもできます。

このレッスンでは、次の操作を行います。

- エージェントページで cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

ハイコントラストモードをエージェントに依頼した後、チームはライトモードも追加したいと考えました。サイトのスタイルと切り替え機能をすでに変更しているため、同じ作業に含めるのが合理的です。エージェントにライトモードも追加するよう指示します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てられたタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左ペインの両方にタスクが表示されます。次のようなタスクが含まれていることを確認します。
   - コードベースのドキュメント更新。
   - ゲーム変更用 API の生成。
   - Web サイトへのハイコントラストモードの追加。
3. 実行中のタスクを選択し、Copilot が実行した次のような処理を確認します。
   - リポジトリのチェックアウト。
   - Copilot の作業環境の作成。
   - MCP サーバーのセットアップ。
   - 割り当てられたタスクを完了するための処理。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更されたファイルを確認できます。

## cloud agent に追加の指示を送る

アクティブなタスクを確認できました。次は、ハイコントラストモードを実装中の Copilot にライトモードも追加するよう依頼します。

1. ハイコントラストモードを追加するセッションを選択します。タイトルは Copilot が選んだ名前や作業状況によって異なります。

    ![ミッションコントロール内のアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![エージェントページで、作業中の cloud agent に指示するダイアログを強調表示したスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot が指示を受け取り、作業に反映することを確認します。

## Copilot の作業を待つ

Copilot は更新されたタスクに取り組みます。送信時に進めていた手順が終わると、新しい要望を作業に取り込みます。

今回も数分かかります。その間に休憩し、ここまでに学んだことを振り返りましょう。

## まとめと次のステップ

Copilot cloud agent のタスクを監視し、方向を示すための中心的な場所である Copilot agents ページを確認しました。進行中および完了したタスクを追跡し、作業内容をレビューして、進行中のタスクに追加の指示を送れます。

このレッスンでは、次の操作を行いました。

- ミッションコントロールとエージェントページで cloud agent のタスクを監視しました。
- 進行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を作成している間に、次は [Copilot が作成したプルリクエストを改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
