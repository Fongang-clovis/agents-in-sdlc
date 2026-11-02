---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの小さな更新でも、通常は複数のファイルを変更し、テストを実行する必要があります。開発者は、関連ファイルを見つけ、変更を加え、テストを実行し、デバッグし、見落としたファイルを特定して再度変更します。このような繰り返し作業に役立つのが Copilot Agent Mode です。

Copilot Agent Mode は IDE 内でより自律的に作業します。開発者のように既存のプロジェクト構成を調べ、必要なファイルを更新し、テストなどのタスクを実行し、問題があれば自動的に修正します。Agent Mode を使ってサイトに新しい機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途向けの機能です。agent mode は IDE 内で作業し、すばやいフィードバックと対話を可能にします。cloud agent はチームメンバーのように非同期で作業し、issue とプルリクエストを通じてやり取りします。

この演習では、次の操作を行います。

- Copilot Agent Mode でプロジェクトを探索し、関連ファイルを特定して、複数のファイルをまとめて変更する方法を確認します。
- UI とデータレイヤーにまたがる新機能を実装します。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューします。

## シナリオ

ゲームの数が増えているため、カテゴリと発行者で絞り込めるようにします。前の演習では出版社ヘルパーを追加しました。次は Copilot Agent Mode で残りのデータレイヤー、UI、テストを完成させます。

## Tailspin Toys の Web サイトを実行する

変更前に、現在の機能を確認します。Web サイトは開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲーム一覧と詳細を表示する単一の Astro アプリで、ビルド時に静的 HTML を生成します。ページは frontmatter 内で Drizzle ORM を使ってローカル SQLite データベースを直接検索します。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、インタラクティブな機能は標準 DOM API を使う小さな Astro `<script>` で追加します。

### Web サイトを起動する

`npm run dev` タスクで単一の Astro 開発サーバーを起動できます。

1. Codespace に戻ります。前の演習の後、`main` に戻っているはずです。
2. Codespace で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. フィルター作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルの `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動転送されます。これは Web サーバーをホストする Codespace へのプライベートトンネルです。

### Web サイトを確認する

主な機能を確認します。

- **ホームページ:** ボードゲームのタイトル、画像、説明を一覧表示します。
- **ゲーム詳細ページ:** ゲームを選択すると、タイトル、説明、発行者、カテゴリなどの詳細が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の操作を始める前に、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なるため、該当する箇所で説明します。

最初の Web サイトは動作していますが、さらに機能を追加します。テンプレートからプロジェクトを作成したときに issue のバックログも作成されているので、GitHub Copilot に内容を表示してもらいます。

1. Chat ビューのエージェント選択メニューで **Agent** を選びます。Agent は複数ファイルにまたがる変更を計画して実装し、ターミナルコマンドやツールを実行できます。

   ![Chat ビューのエージェント選択メニューのスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 使用可能なモデルから **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者がモデルを推奨しているわけではありません。ワークショップの作成時に Claude Sonnet 4.5 を使ったため、比較的安定した体験を得られるよう手順に記載しています。LLM は確率的に動作するため、表示される提案が異なる場合があります。これは通常の動作です。

> [!NOTE]
> Copilot が別の MCP コマンドを使う場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信して、バックログの issue を確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. すべての issue を一覧表示するコマンドを実行するには **Continue** を選択します。
5. issue の一覧を確認します。

Copilot がユーザーに最も役立つ内容に基づいて issue の優先順位も決めることを確認します。

## 指示ファイルを確認する

コード生成を開始する前に、作業のコンテキストを Copilot に与える指示ファイルを確認します。ここでは、Web サイトに機能を追加するときの方法を説明する [UI 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関するガイダンスを確認します。アーキテクチャの概要、コンポーネントの設計、テスト容易性とアクセシビリティの原則、Astro、Tailwind CSS、Drizzle などの指示ファイルへのリンクが含まれます。

> [!TIP]
> 指示ファイルから、他の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。再利用を促し、大きな指示を管理しやすく分割し、コード生成の例やテンプレートを示せます。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも次の3種類の変更が必要です。

- データレイヤー (`src/lib/`) にフィルター処理を追加または調整します。
- フィルター動作のテストを追加または更新します。
- ゲーム一覧ページにフィルター UI を追加します。

さらに、コードベースにマージする前にテストを実行して成功させる必要があります。Copilot Agent Mode に依頼しましょう。

1. 現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント選択メニューで **Agent** が選択されていることを確認します。
4. モデルが **Claude Sonnet 4.5** であることを確認します。
5. バックログの関連 issue に基づいて機能を実装するよう、次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、関連するファイルを特定して変更し、テストを実行する様子を確認します。

   ![Copilot がプロジェクトを調べるスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行うため、数分かかる場合があります。作業中はエディターに **Keep** と **Undo** が表示されることがあります。作業が完了するとすべての変更に対して操作できるため、進行中に選択する必要はありません。

7. Copilot に求められたら **Continue** を選択してテストを実行します。

   ![Copilot Chat でテスト実行の確認を求めるダイアログのスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。Copilot はコード生成とテストを反復し、エラーがないと判断するまで作業を続けます。

   ![Copilot Agent Mode の完了したチャットセッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新機能を試します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode で Tailspin Toys に機能を追加しました。この演習では、次のことを学びました。

- UI とデータレイヤー全体に新機能を実装する方法。
- Copilot Agent Mode でプロジェクトを探索し、関連ファイルを特定して、複数のファイルをまとめて変更する方法。
- コードベースにマージする前に生成された変更とテストをレビューする方法。

次は、[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加の演習 — ページネーションを実装する

ゲーム一覧が増えるとページネーションが必要になります。この演習で学んだ方法を使い、Copilot にページネーションの実装を依頼します。既存の指示ファイルを利用する、ページサイズをユーザーが選ぶか固定するかを決める、期待する結果に必要なガイダンスをプロンプトに含める、結果を確認して追加の指示を送る、といった点を考慮してください。Copilot にコンテキストを伝えて改善するのは通常の作業です。

## リソース

- [Copilot の ask、edit、agent mode の役割と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
