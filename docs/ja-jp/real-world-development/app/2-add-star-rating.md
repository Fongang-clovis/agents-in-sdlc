---
title: "レッスン 2 - 星評価の追加で小さな成果を得る"
description: "GitHub Copilot app で最初のエージェントセッションを開始し、ゲームカードに小さな変更を加えて、最初の pull request をマージします。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

ワークスペースを確認してクイックチャットを使ったので、次は**エージェントセッション**を開始し、プロジェクトに最初の変更を加えます。変更は小さくします。ゲームのデータにはすでに星評価がありますが、ホームページのゲームカードにはまだ表示されていません。エージェントに表示を依頼し、変更をレビューして、最初の pull request としてマージします。

このレッスンでは、次の操作を行います。

- エージェントセッションを開始し、セッションの構成を学びます。
- プロジェクトに小さく対象を絞った変更を加えるようエージェントに依頼します。
- ワークスペースの差分ビューで変更をレビューします。
- アプリをローカルで実行し、ブラウザーで変更を確認します。
- 最初の pull request を作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲームの詳細ページにはすでに表示されています。しかし、ホームページのゲームカードに表示されるのはタイトル、カテゴリ、発行者、説明だけです。ウォームアップとして、各カードに既存の評価を表示するようエージェントに依頼します。これは小規模で自己完結した変更なので、最初のセッションに適しています。

## セッションの構成

**セッション**は、独立したワークスペースで実行されるエージェントとの会話です。各セッションには**専用の git worktree とブランチ**が割り当てられます。そのため、一方では機能を追加し、もう一方ではバグを修正するなど、変更を競合させずに複数のセッションを同時に実行できます。セッションはリポジトリごとにグループ化されてサイドバーに表示されます。いずれかを選択すると、そのセッションに切り替わります。

セッション内には、エージェントとの**会話**、ファイルを調査および編集するときのエージェントの**ツールアクティビティ**、差分付きの**変更済みファイル**一覧が表示されます。

## セッションを開始して変更を依頼する

新しいセッションを開始して、プロジェクトを調べながら機能を実装します。[アプリのセットアップ][prior-lesson]で、GitHub リポジトリからプロジェクトを追加しました。そのリポジトリに新しいセッションを作成して変更を依頼します。

1. GitHub Copilot app に戻るか、アプリを開きます。
2. サイドバー上部の **+** を選択してセッションを開始します。
3. リポジトリとして `tailspin-toys` を選択します。
4. プロンプトボックスの下で **new working tree** と **Interactive** モードを選び、次のプロンプトを送信します。

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. <kbd>Enter</kbd> を押して、プロンプトを Copilot に送信します。

Copilot app は最初にプロジェクトの分離されたコピーである新しい worktree を作成します。次にプロジェクトを調べ、新機能に必要なファイルを見つけてコードを作成します。これで Copilot app を使って新しい機能を追加できました。

## 差分をレビューする

AI が生成した変更は、小さなものも含めてマージ前にレビューしてください。Copilot app 内で変更を確認します。

1. アプリの右上にある **Toggle review panel** を選択します。開いたパネルで **Changes** を選択し、変更されたファイルと差分を確認します。

    ![Create PR の右側にある Toggle review panel ボタンを矢印で示した GitHub Copilot app の上部ツールバー](../../../_images/app-2-review-panel.png)

2. ゲームの詳細を表示する中心的なファイル `GameCard.astro` にコードが追加されていることを確認します。評価がある場合は表示し、`starRating` が `null` の場合は "No rating yet" を表示する、次のような小さなブロックになるはずです。

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Copilot を含む生成 AI ツールは決定論的ではなく確率的に動作するため、生成コードは上記と異なる場合があります。似た結果になっているはずです。

## 変更を確認する

ブラウザーを開く前に、エージェントの自動チェック結果をレビューします。数値の `starRating` と、`null` の場合の代替表示をテストが確認していることを確かめます。前提条件が不足していたり、チェックがスキップされていたりする場合は成功ではありません。インストールの要求は、内容を確認してから承認してください。

コードを読むだけで動くと判断せず、Copilot に Web サイトを起動させ、ブラウザーキャンバスで更新された UI を確認します。

> [!TIP]
> キャンバスは、Copilot app 内で利用できるインタラクティブなウィジェットです。後のレッスンではカスタムキャンバスを確認して自分でも作成します。ここでは組み込みのブラウザーキャンバスを使います。

1. 次のプロンプトを送り、アプリを起動してブラウザーキャンバスで開くよう依頼します。

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. しばらくするとアプリが起動し、Copilot app 内にブラウザーウィンドウが開きます。
3. 評価済みのゲームカードに5点満点の値が表示されることを確認します。
4. 確認が終わったら、次のプロンプトで、このセッションに対して起動した開発サーバーを停止するよう依頼します。

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## 最初の pull request を作成してマージする

機能を追加できました。次は、新しいコードを既存のコードベースにマージする pull request (PR) を作成します。

1. 右上隅の **Create PR** を選択します。
2. 認証を求められた場合は **Sign in with your browser** を選び、画面の指示に従います。
3. リポジトリのポリシーにより、PR の作成前に関連する issue が必要だと Copilot が説明した場合は、その手順に従うよう確認します。
4. Copilot が PR を作成します。
5. チャットのすぐ上にある **PR** バブルを選択し、レビューペインで PR を開きます。必要に応じて内容をレビューします。
6. 問題がなければ **Ready to merge** を選択します。
7. 新しいダイアログで **Merge pull request** を選択して、PR をマージします。

## まとめと次のステップ

GitHub Copilot app を使って最初の変更をリリースしました。具体的には、次の操作を行いました。

- エージェントセッションを開始し、セッションの構成を学びました。
- ゲームカードに小さく対象を絞った変更を加えるようエージェントに依頼しました。
- ワークスペースの差分ビューで変更をレビューしました。
- アプリをローカルで実行し、ブラウザーで星評価を確認しました。
- PR 1 を作成し、チェックをレビューしてから明示的にマージしました。

次は、[フィルター機能の issue から始めて Plan と Autopilot モードを使い][next-lesson]、より大きな機能を構築します。

## リソース

- [GitHub Copilot app でのエージェントセッションの操作][agent-sessions]
- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app で issue とプルリクエストを管理する][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#github-copilot-app-をインストールして構成する
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
