---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で方針を決め、Autopilot で issue からフィルター機能を実装し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初にプロジェクトへ小さな機能を追加しました。より大きな変更には、さらにしっかりしたプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに沿って作業できるため、適切な方法で必要な機能を構築できます。このレッスンから数回にわたり、issue から新機能を作り、コードの妥当性と機能の動作を確認して、最終的にプロジェクトへマージする一般的なエージェント駆動の開発プロセスを進めます。

> [!NOTE]
> 機能開発を続ける間は、同じ会話とブランチを使います。通常はファイル形式ごとにブランチや PR を分けますが、ここでは主な概念に集中できるように手順を簡略化します。

このレッスンでは、次の操作を行います。

- GitHub issue から Copilot の新しい会話を開始します。
- Plan モードで要件を定義します。
- Autopilot モードで機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで手動検証します。

この後のレッスンでは、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成したうえで、機能の PR を作成します。

## シナリオ

Tailspin Toys のカタログが増えたため、訪問者がゲームをカテゴリや発行者で絞り込めるようにする必要があります。バックログの issue には機能が記載されていますが、カテゴリを組み合わせる方法など、コーディング前に合意が必要な詳細もあります。Plan モードで判断を整理し、範囲が明確な実装を Autopilot に任せます。

## 背景

AI コーディングエージェントを開発フローに導入しても、基本は変わりません。むしろ、基本がさらに重要になります。多くの開発者は、次のような流れで作業します。

1. 必要な作業を記載した issue から始めます。
2. 実装する内容を計画します。
3. コードを構築してレビューします。
4. テストを実行してコードを検証します。
5. 新しい機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 詳細はチームや組織によって異なりますが、多くの場合は上記の流れを調整したものになります。

この標準的な方法を守ることで、AI が生成したコードが要件を満たし、手作業で書いたコードと同じレビュー工程を経るようにできます。

## 会話モード

**会話モード**は、エージェントがどこまで自律的に作業するかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すと、モードを切り替えられます。

- **Interactive:** エージェントと一緒に作業します。エージェントは変更を提案し、次に進む前に入力を待ちます。
- **Plan:** エージェントが先に計画を作成し、プロジェクトファイルの編集を行いません。
- **Autopilot:** エージェントが自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## issue から始める

フィルター機能の作業を始める前に、Codespace とターミナルが準備できていることを確認します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージされていることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新して、フィルター作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を2回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーを使って **Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7`（または同様の番号）が追加されます。`#` を使うと、GitHub 上の issue やプルリクエスト (PR) をコンテキストに含められます。

## フィルター機能を計画する

計画を立てることで、機能の実装やタスクの実行を Copilot に任せる前に、方針を決められます。複雑な作業は時間をかけて計画することをお勧めします。Plan モードに切り替え、Copilot に計画の作成を依頼します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下に **Plan** と表示されていることを確認します。
2. 前の手順で追加した issue の参照に続けて、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、適切な方法を検討して計画を作成します。

3. Copilot がフィルターの動作について質問する場合は、希望に応じて回答します。正解は1つではありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して展開します。
5. 計画を上から下まで確認します。
6. 判断内容と異なる部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画が完成してレビューも終わったので、実装に進みます。Autopilot を使って Copilot に作業を任せます。Autopilot では、Copilot はタスクが完了したと判断するまで問題に取り組み続けます。

1. **Accept plan and build on autopilot (recommended)**（またはインストール済みのバージョンで同様のラベルの項目）を選択します。
2. プロンプトの下に **Autopilot** と表示されていることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装が始まることがあります。必ず計画を先に確認してください。依存関係が不足している、またはポートが競合していると Copilot が報告した場合は、チェック完了とみなす前にセットアップの問題を解決します。

## 実装をレビューして検証する

コードの生成後は、他のコードと同じようにマージ前にレビューします。コードを確認し、サイトを起動して動作も確かめます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力し、フィルターの実装とテストを確認します。
3. issue と計画時に決めた内容に照らし合わせます。
4. コードをレビューしたら <kbd>Esc</kbd> を押して差分表示を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新しい機能を試す

コードに問題がなさそうでも、実際に動くか確認しましょう。前と同じようにアプリを起動し、Codespaces で転送されたポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能と表示されたら、**Open in Browser** を選択します。
3. 計画時に合意したカテゴリ、発行者、それらを組み合わせたフィルターを試します。
4. リセットと検索結果がない場合の動作が、issue と合意内容に沿っていることを確認します。
5. Codespace に戻り、起動した開発サーバーを停止するよう Copilot に依頼します。

## まとめと次のステップ

異なる会話モードを使って機能を構築し、レビューしました。このレッスンでは、次の操作を行いました。

- GitHub issue から Copilot の新しい会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動検証しました。

次は、[カスタム指示を使って][next-lesson]コードを文書化された規約に従わせる方法を詳しく確認します。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
