---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Copilot CLI を使い、Tailspin Toys で一連のワークフローを進めました。次の操作を行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを確認し、用意されているフィルター機能の issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター機能の issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- quality assurance (QA) カスタムエージェントを作成して選択し、要件、テスト範囲、スキルの結果、ブラウザーでの検証結果を評価しました。
- フィルター機能の変更全体をレビューし、Agent Merge を許可してフィルター機能の PR をマージしました。
- コンテキスト、モデル、共有、任意のクラウド委任に使うスラッシュコマンドを確認しました。

## リリースした内容

ワークショップには、更新された `main` からそれぞれ独自のブランチで進める、2つの PR の節目があります。

1. **星評価:** ゲームカードに、既存の `starRating` と未評価であることを明示する表示を追加します。
2. **フィルター機能と品質ワークフロー:** フィルター機能を実装し、指示を更新してその機能に適用します。`quality-checks` レポートをカスタマイズし、QA プロファイルを作成して関連テストを追加します。

フィルター機能の計画から PR の作成までは同じ会話とブランチを使いました。ワークショップを簡潔に進めるため、作業を1つの PR にまとめました。

## 検証の種類

自動テスト、自分で行うブラウザー確認、MCP を介した Copilot のブラウザー操作という複数の方法でコードを確認しました。`quality-checks` スキルは単体テスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA エージェントは、それらの結果に要件とテスト範囲のレビューを加えて PR を確認しました。

追加したテストで実際の不足が解消されることが重要です。QA の実行で新しいテストが不要と判断される場合もあります。マージを許可する前にコードと検証結果を確認し、変更後は影響を受ける検証結果を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業内容を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP サーバーを設定しました。会話をまたいでカスタマイズを再利用し、チームのニーズに合わせて調整してください。指示は基準を設定し、スキルは繰り返し可能なタスクを説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った **モードとモデル** を選びます。構築の前に方針を考えるには **Plan**、範囲を絞った変更で確認を続けるには **Interactive**、範囲が明確なタスクには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業には高性能なモデルを選びます。

インフラストラクチャと同じように、コンテキストも重要です。何を、なぜ、どのように作りたいかを明確に伝えることで、出力が大きく変わります。

## さらに確認する

基本的なワークフローを学びました。Copilot CLI には、次のような機能もあります。

- `/review` を使い、コードレビューエージェントに変更を分析してもらいます。
- `/rubber-duck` を使い、問題について対話して別の視点を得ます。
- `/fleet` を使い、独立したサブタスクを並行して調整します。
- `/worktree` を使い、別のタスクを分離します。
- `/delegate` を使い、Copilot cloud agent にタスクを送信します。

## 次のステップ

ツールのスキルを伸ばす最善の方法は、使い続けることです。実運用のコード、趣味のコード、以前から作りたいと思っていた小さなアプリなどで活用してください。学んだことをチームで共有し、互いの経験から学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張したい場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進んでください。別の環境を比較する場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

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
