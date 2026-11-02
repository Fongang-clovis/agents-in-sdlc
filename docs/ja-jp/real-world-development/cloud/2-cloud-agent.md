---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

多くの組織では、未解決のセキュリティ問題、更新が必要なレガシーコード、実装の時間を確保できずにバックログに残った機能要望など、技術的負債に悩んでいます。GitHub Copilot cloud agent は、コードの更新や機能の追加などの作業を自律的に実行できます。作業が終わると、人間の開発者がレビューできるドラフト PR を作成します。時間のかかる作業を委任し、開発者はより大きな課題に集中できます。

この演習では、Copilot cloud agent で次のことを確認します。

- コード生成環境をカスタマイズする方法。
- 操作を安全に行う方法。
- issue の範囲を明確にする重要性。
- issue を Copilot に割り当てる方法。

## シナリオ

Tailspin Toys では技術的負債に対処する必要があります。最初のサイトを作成した委託先がドキュメントを残しておらず、アプリケーションのエクスポート関数に TSDoc コメントを追加したいと考えています。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する関連ゲームを表示し、UX の受け入れテストができる状態にしたいと考えています。実装を洗練させる必要はありませんが、作業の優先度は他の issue より低くなっています。

どちらも、優先順位が下がって対応されないままになりやすいタスクです。Copilot cloud agent に割り当てれば、別の作業を進めている間に非同期で処理され、後から結果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドで作業できます。たとえば、[GitHub issue を Copilot に割り当てる][assign-issue]方法があります。割り当てると Copilot は進捗を追跡するドラフトプルリクエストを作成し、環境を準備してタスクに取り組みます。作業中または完了後にセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。単純に見える操作でも、詳しく見ると複雑な場合があります。[Copilot cloud agent のタスク範囲を明確に][cloud-agent-best-practices]してください。AI ペアプログラマーとして、段階的に作業し、学び、試し、必要に応じて調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示が Copilot の作業に役立つことを確認しました。まだ `.github/copilot-instructions.md` を確認していない場合は、cloud agent に作業を割り当てる前に、リポジトリに含まれる指示ファイルを読み取り専用で確認しておきます。後で結果にどのように影響したかを確認できます。

GitHub Web UI または起動済みの Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TSDoc コメントに関する指針を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` frontmatter が `**/*.test.ts` に適用されることを確認します。

次のセクションで **Code lacks documentation** issue を cloud agent に割り当てます。生成されるプルリクエストでは、これらの指示に基づく TSDoc コメント、TypeScript の規約、コメントヘッダーを確認してください。後の演習で PR をレビューするときにも触れます。

## Copilot cloud agent の開発環境を設定する

コード作成には、通常ツールや依存関係をインストールする環境が必要です。Copilot にタスクを割り当てる場合も同様です。cloud agent は [GitHub Actions][github-actions] を使って作業環境を準備します。`.github/workflows/copilot-setup-steps.yml` に設定する[セットアップワークフロー][setup-workflow]を作成すると、必要な開発ツールや依存関係を利用できるようになります。このラボには、Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、単一 Astro アプリ用にマイグレーションと初期データを設定した SQLite データベースが準備されています。

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同じですが、重要な点があります。

- `copilot-setup-steps` という単一のジョブがあり、Copilot が PR 作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` により、リポジトリの **Actions** タブから手動で実行できます。Copilot を待たずにワークフローをテストできます。

## ドキュメントを追加する

多くのプロジェクトは情報が古いか、ドキュメントが不足しています。こうした技術的負債は生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメント作成が得意なので、cloud agent に割り当てるのに適した issue です。後の演習で作業結果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブ、**New issue**、**Blank issue** の順に選択します。
3. **Title** に `Code lacks documentation` と入力します。
4. **Description** に次を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

5. **Create** を選択して issue を作成します。
6. 右側の **Assign to Copilot** を選択し、ダイアログで **Assign** を選択します。

  ![issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

  ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

7. **Pull Requests** タブを選び、作成された PR を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。まだ表示されない場合は少し待ってからブラウザーを更新します。
8. 数分後、Copilot が TODO リストを作成します。

> [!NOTE]
> TODO リストが PR に表示されるまで数分かかることがあります。Copilot は前述のワークフローを実行して環境を準備し、プロジェクトを分析して、作業方針を決めています。

9. TODO リストと作業内容を確認します。
10. プルリクエストのタイムラインを下にスクロールし、Copilot が issue に取り組み始めたことを確認します。
11. **View session** を選択します。

  ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するために、ウィンドウの再読み込みが必要な場合があります。

12. ライブセッションで、Copilot がコードを調べて状態を把握し、計画を考え、コードを作成する様子を確認します。

非同期に作業することは cloud agent の主な利点の1つです。別の作業をしながら待つため、次の関連ゲームの issue も cloud agent に割り当て、その間にアプリケーションの別機能に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

cloud agent に作業を分けて任せている間に、別の作業を進められます。関連ゲームのセクションを作る作業に時間がかからなくても、その時間を別の作業に使えます。次の issue を Copilot cloud agent に割り当てます。

1. github.com のリポジトリを開き、**Issues**、**New issue**、**Blank issue** の順に選択します。
2. **Title** に `Show related games on the game details page` と入力します。
3. **Description** に次を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

4. **Create** を選択し、右側の **Assign to Copilot** を選択してから **Assign** を選択します。

issue の最初のコメントに 👀 が表示され、Copilot が作業中であることを示します。

![issue で作業中であることを目の絵文字で示す Copilot](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼に取り組んでいます。cloud agent はソフトウェアエンジニアのように作業するため、常に監視する必要はありません。作業完了後にレビューします。次はカスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を使うと、issue を Copilot に割り当てて非同期に処理できます。技術的負債への対応、新機能の作成、フレームワーク間のコード移行などに活用できます。

この演習では、次のことを確認しました。

- コード生成環境をカスタマイズする方法。
- 操作を安全に行う方法。
- issue の範囲を明確にする重要性。
- issue を Copilot に割り当てる方法。

cloud agent がバックグラウンドで作業している間に、次はカスタムエージェントを作成して使います。[Copilot cloud agent は MCP サーバー][cloud-agent-mcp]とカスタム指示も利用できます。これらは前の演習で確認しました。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
