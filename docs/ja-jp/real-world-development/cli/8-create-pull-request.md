---
title: "レッスン 8 - 機能 PR の作成とマージ"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、指示の更新、スキルの更新、quality assurance (QA) プロファイル、テストがすべて1つのブランチに保存されています。まとめてレビューし、プルリクエストを作成しましょう。星評価のプルリクエスト (PR) は手動でマージしましたが、今回は **Agent Merge** に処理を任せます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。このワークショップでは手順を簡潔にするため、フィルター機能と品質ワークフローを1つの会話とブランチで進め、この PR にまとめました。

このレッスンでは、次の操作を行います。

- Agent Merge と、マージまでの処理を自動化する方法を学びます。
- 機能の変更全体と検証結果を確認します。
- フィルター機能の PR を作成します。
- レビュー後に Agent Merge を有効にして、PR がマージされたことを確認します。

## シナリオ

フィルター機能のワークフローを通じて、Copilot で機能の計画、実装、検証を行いました。Tailspin Toys では、開発者がマージを許可するタイミングを管理しながら、残りの PR 作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は、プルリクエストをマージするまでに必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げている問題（失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要なリベースなど）に取り組みます。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

ここまでは **Merge pull request** を自分で選択してきました。Agent Merge にその処理を任せることもできます。Agent Merge を有効にする前に、作業をレビューして準備ができていることを確認してください。

## Agent Merge で PR を管理する

コードの作成が完了したので、すべてをレビューして PR を作成し、残りの処理を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. 選択肢から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを使って新しい PR を作成します。
5. `/pr agentmerge` コマンドを使って Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーション処理を確認します。すべて成功すると、マージが実行されます。
7. 「PR #14 was squash-merged successfully.」のような Copilot のメッセージを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限の不足を回避しません。処理がブロックされた場合は、報告された理由を確認し、リポジトリで許可されている場合はレビュー済みの変更を手動でマージしてください。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストの処理など、開発プロセスの複数の手順を自動化しました。具体的には、次の操作を行いました。

- Agent Merge と、マージまでの処理を自動化する方法を学びました。
- 機能の変更全体と検証結果を確認しました。
- フィルター機能の PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、任意のクラウド委任に使う[便利な Copilot CLI スラッシュコマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
