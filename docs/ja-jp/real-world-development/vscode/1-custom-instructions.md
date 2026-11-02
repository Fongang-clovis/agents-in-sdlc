---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を利用するときは、コンテキストが重要です。特定の方法で作業する必要がある場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot と特定のコンテキストを共有する方法はいくつかあります。重要な方法の1つが[指示ファイル][instruction-files]です。指示ファイルでは、コード生成の指針を伝えます。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルによって、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント規約が Copilot に伝わる方法を確認します。
- 既存の指示を使って、フィルター機能のデータ層に最初の出版社ヘルパーを作成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する基準を追加します。
- 同じプロンプトを再び使い、生成コードが新しい基準に沿うことを確認します。
- 指示とフィルター機能の変更を `main` にコミットし、次の演習で更新された指示を使えるようにします。

> [!CAUTION]
> 生成コードが設定した規約の一部に従わない場合があります。Copilot の出力は確率的です。この演習では、文字単位で同じ出力を得るのではなく、指示の更新後に全体的な傾向が変わることを確認します。

## シナリオ

Tailspin Toys には開発のガイドラインと要件があります。たとえば、次のとおりです。

- データレイヤーには必ず単体テストを用意します。
- UI はダークモードにし、現代的な見た目にします。
- TSDoc のドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、Copilot がこれらの開発方針に沿って作業できるようにします。

## 指示ファイル

カスタム指示は、コーディングスタイルや要件を Copilot に伝えるコンテキストとして使えます。ライブラリ、規約、コードコメントの書き方などを指定し、より適切な提案やコード生成につなげられます。リポジトリ全体または特定のファイル形式や作業に対して指示を作成できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に渡す単一の指示ファイルです。技術スタック、開発中の内容、ベストプラクティスなど、ほとんどのチャットや CLI リクエストに関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル形式、作業向けの指示です。たとえば TypeScript や Astro、UI コンポーネントの作成、単体テストの追加などに使います。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示と `applyTo` frontmatter を持つ `*.instructions.md` ファイルを使ってコードを生成します。
>
> Copilot は AGENTS.md や CLAUDE.md など、他の規格に基づく指示ファイルもサポートしています。[サポートされるカスタム指示][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成について詳しく説明することは、このワークショップの範囲外です。サンプルプロジェクトの例から、基本を確認します。

- `copilot-instructions.md` には、プロジェクトの説明、構成、全体的なコーディング規約などの情報を記載します。
- `*.instructions.md` には、単体テスト、Astro コンポーネント、データレイヤーなどのファイル形式や、特定の作業についての指示を記載します。
- 自然な言葉を使い、明確な指針とコードの良い例・悪い例を示します。

指示ファイルの作成方法は1つではありません。AI と同じように、実際に試し、プロジェクトやシナリオに合わせて調整してください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意してください。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまな作業向けの指示があります。
>
> Copilot に指示ファイルを生成してもらうこともできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` など、画面ごとに方法が異なります。該当するレッスンで説明します。
>
> テンプレートや出発点には、指示ファイルやカスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] を利用できます。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization

## このプロジェクトのカスタム指示を確認する

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの説明と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などを確認します。**Code standards** 内の **GitHub Actions Workflows** の指針も確認してください。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle データレイヤー、テストなどの指示ファイルを確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` には、指示が適用されるファイルを指定する glob が記載されています。たとえば `**/*.test.ts` に一致する TypeScript のテストファイルに適用されます。
5. プロジェクトで単体テストを作成するときの指示を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾を確認します。他の指示ファイルや既存ファイルへのリンクがあり、大きな指示を小さな再利用可能ファイルに分け、参考例を指定できます。リンクのパスは指示ファイルを基準とします。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング規約がありますが、コード内のドキュメントはまだ求められていません。次の手順で TSDoc コメントとファイルヘッダーの規則を追加します。

## 変更用ブランチを作成する

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の操作を始める前に、Codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なるため、該当する箇所で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。その後、同じプロンプトを再送信して違いを確認します。

1. 前の演習で開いたエディタータブを閉じ、Copilot に必要なコンテキストだけを残します。
2. `src/lib/publishers.ts` を開き、ヘルパーを置く場所を Copilot に示します。
3. Chat ビューのエージェント選択メニューで **Agent** を選び、ファイルを変更できるようにします。

   ![Chat ビューのエージェント選択メニューのスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを確認してコードを更新します。ヘルパーファイルとテストが変更されることがあります。
6. 生成されたヘルパーが、最初の引数に `db` クライアントを受け取り、出版社の型付き配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約に基づきます。
7. 生成されたコードに TSDoc コメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加することがあります。その場合も問題ありません。この演習では、指示を更新した後に一貫性が高まることを確認します。

## リポジトリ全体に新しい基準を追加する

1. `.github/copilot-instructions.md` を開き直します。
2. **Code formatting requirements** セクション（27行目付近）を探します。既存の規約はありますが、コード内のドキュメントを求める規則はありません。
3. 既存の規約の直後に次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> リポジトリ全体の指示は `.github/copilot-instructions.md` に、言語やファイル形式、タスク固有の指示は `*.instructions.md` に記載できます。ここで追加するようなプロジェクト全体の基準には、リポジトリレベルのファイルが適しています。

## プロンプトを再送信して変化を確認する

1. Copilot Chat に戻って **New Chat** を選び、会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot が対象ファイルに注目できるようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 生成されたファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 関数に次のような TSDoc コメントがあることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト規約を Copilot に伝え、次の演習で使うコードに適用できました。

## 指示とブランチをコミットしてプッシュする

指示ファイルも他のファイルと同様にソース管理します。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
2. ターミナルで変更を確認します。

   ```bash
   git status
   ```

3. 指示の更新とヘルパー作成に Copilot が加えたファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、指示の更新に関するバックログの issue と関連付けたプルリクエストを作成します。Copilot に関連する issue を見つけ、PR を作成してマージするよう依頼します。

> [!NOTE]
> Copilot が GitHub 上の操作を行う最初の演習です。プロジェクトテンプレートには GitHub のツールが構成されているため、追加のセットアップは必要ありません。Copilot が初めてツールを使うときに VS Code でサインインを求められた場合は、画面の指示に従ってください。この接続方式 (Model Context Protocol) については、後の演習で説明します。

1. Codespace の Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** を選択します。
3. 指示ファイルの更新と、次のフィルター作業に使う出版社ヘルパーの土台の両方を説明する PR を現在のブランチから作成するよう、Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が GitHub の操作を許可するよう求めたら、コマンドを確認して必要なものを **Allow** します。
5. PR が作成されたら、次のプロンプトで PR のマージと `main` への復帰を依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. GitHub の操作とシェルコマンドの実行を許可するよう求められたら、内容を確認して必要なものを許可します。

GitHub Copilot の支援でプルリクエストを作成してマージできました。

## まとめと次のステップ

この演習では、Copilot が指示ファイルからコンテキストを取得する方法を確認し、VS Code の Copilot Chat で次の操作を行いました。

- 現在の指示を使ってコード生成プロンプトを送信し、生成内容を確認しました。
- `.github/copilot-instructions.md` に新しいリポジトリ全体の基準を追加しました。
- 同じプロンプトを再送信し、生成コードが新しい基準に従うことを確認しました。
- 指示の更新とヘルパーの土台を `main` にコミットし、次の演習で使えるようにしました。

次は、Copilot がコードベース全体に機能を追加する[agent mode][next-lesson]で指示を活用します。

## リソース

- [GitHub Copilot のカスタム指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示をより良く書くための5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
