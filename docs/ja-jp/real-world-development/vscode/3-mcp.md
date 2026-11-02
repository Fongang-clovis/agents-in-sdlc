---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

Agent mode でフィルター機能を作成しました。プルリクエストを開く前に、ブラウザーで実際に動くことを確認します。自分でアプリを操作する代わりに **Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストしてから、ブランチを公開して PR を作成します。

この演習では、次の操作を行います。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する方法を学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot にサーバーを使わせ、ブラウザーでフィルター機能を手動テストします。
- ブランチを公開して、フィルター機能のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エディターの外部に接続できると、agent mode はより強力になります。Model Context Protocol (MCP) は、Copilot が外部のツールやサービスと通信するための標準です。

![agent mode、コンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールの関係を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部ツールやサービスと通信できます。最新情報にアクセスしたり、ユーザーに代わって操作を実行したりできます。

MCP サーバーは AI エージェントと外部ツールやサービスの橋渡しをします。既存の API や npm パッケージなどのローカルツールとの通信を管理し、AI エージェントが利用できるツールとリソースを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスし、リポジトリの作成、更新、issue やプルリクエストの管理などを行えます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright でブラウザーを自動操作し、ページの移動、フォームの入力、ボタンの選択などを行えます。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステム内のサーバーを見つけて貢献できるように [MCP registry](https://github.com/mcp) を公開しています。

> [!CAUTION]
> MCP サーバーは、プロジェクト内の他の依存関係と同様に扱ってください。利用前にソースコードを確認し、発行元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP 構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. 次の `github` サーバーがすでに設定されています。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクトテンプレートに含まれています。そのため、前の演習で Copilot がバックログの issue を読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うので、ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot がツールを初めて使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

[Playwright MCP サーバー][playwright-mcp-server]は、Copilot が操作できるブラウザーを提供します。これを使って機能をテストします。

1. `.vscode/mcp.json` の `github` と並べて `playwright` エントリを追加し、次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` はローカルサーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` は画面にブラウザーを表示せずに実行する設定です。デスクトップ環境がない Codespace ではこの設定が必要です。

> [!NOTE]
> Tailspin Toys はエンドツーエンドテストで Playwright を使うため、通常は必要なブラウザーがすでにインストールされています。Copilot がブラウザーがないと報告した場合は、`npx playwright install chromium` を実行してから再試行します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて**起動します。Copilot が初めてサーバーのツールを必要とするとき、VS Code が起動し、信頼するかどうか確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に表示される **Start** を選択して、サーバーを起動し構成を確認します。
2. サーバーを信頼するよう求められたら、構成を確認して信頼する設定を選びます。

手動で起動する必要はありません。省略した場合は、次の手順で Copilot がツールを必要としたときに VS Code が起動し、信頼の確認を求めます。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。エージェントがページを開き、移動し、フィルターを適用し、結果を読み取って要約します。会話を離れずに機能が期待どおり動作するか確認できます。

Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティツリー][playwright-mcp-server]を使います。そのため、支援技術と同じように、ラベル付きのボタン、リンク、リスト項目などの構造化された要素をエージェントが判断します。機能確認と同時に、アクセシビリティの基本的な確認も行えます。

サーバーが接続され、アプリが実行中であることを確認したら、次のプロンプトでフィルター機能を確認します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを使ってブラウザーを起動し、手順を実行して結果を報告します。issue の受け入れ基準と照らし合わせ、問題があれば追加の質問をするか、PR を作成する前にコード修正を依頼します。

> [!NOTE]
> テストするには、アプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止している場合は、プロンプトを送信する前に再起動します。Copilot が初めて Playwright MCP サーバーを使うときにブラウザーがダウンロードされる場合があります。ブラウザーがないという報告があれば、`npx playwright install chromium` を実行してから再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能の動作を確認したので、チームのレビュー用にプルリクエスト (PR) を作成します。最初に `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot による変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミットメッセージを生成します。

   ![Copilot が変更したファイルを示す Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub command-line interface (CLI) など、PR を作成する方法はいくつかあります。ここでは GitHub Copilot に作成を依頼します。Copilot は関連 issue を見つけ、PR と関連付けられます。

1. Copilot Chat パネルで **New Chat** を選び、新しいセッションを開始します。
2. GitHub のツールを使えるように、エージェント選択メニューで **Agent** を選びます。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選び、Copilot が情報を取得して操作を行えるようにします。初めて GitHub ツールを使うときに認証を求められた場合は、画面の指示に従います。
5. Copilot が issue を検索し、適切なものを見つけて PR を作成することを確認します。
6. Copilot が生成したリンクで PR を開きます。ただし、まだマージしないでください。

## まとめと次のステップ

この演習では、次の操作を行いました。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- ブラウザーでフィルター機能を手動テストしました。
- ブランチを公開し、フィルター機能の PR を作成しました。

MCP サーバーを使って機能を**テスト**しました。Copilot を拡張する方法は MCP だけではありません。次は、アクセシビリティレビューなどの作業に使える[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
