---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を利用するときは、コンテキストが重要です。特定の方法で作業する必要がある場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に何を実行するかだけでなく、どのように実行するかも伝えられます。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルによって、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント規約が Copilot に伝わる方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する基準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここではプロンプトの変更前後を比較しません。Copilot cloud agent は GitHub issue を使って非同期に作業するため、リアルタイムで並べて効果を示すのが難しいためです。このハーネスの後半で、cloud agent が作成したプルリクエストをレビューするときに指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には開発のガイドラインがあります。

- データレイヤーには必ず単体テストを用意します。
- UI はダークモードにし、現代的な見た目にします。
- TSDoc のドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、Copilot がこれらの開発方針に沿って作業できるようにします。

### カスタム指示

カスタム指示では、コーディングスタイルや要件に関するコンテキストと希望を Copilot に伝えます。好みの規約、ライブラリ、コードコメントの書き方などを指定し、より適切な提案やコードを生成できるようにします。リポジトリ全体、または特定のファイル形式や作業に対して指示を作成できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に渡す単一の指示ファイルです。技術スタック、開発中の内容、ベストプラクティス、全般的な指針など、ほとんどのチャットや CLI リクエストに関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル形式向けの指示です。TypeScript や Astro などの言語、UI コンポーネントの作成、単体テストの追加などに使えます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示と、`applyTo` frontmatter を持つ `*.instructions.md` ファイルを使ってコードを生成します。
>
> Copilot は AGENTS.md や CLAUDE.md など、他の規格に基づく指示ファイルもサポートしています。[サポートされるカスタム指示][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成について詳しく説明することは、このワークショップの範囲外です。サンプルプロジェクトの例から、基本的な方法を確認します。

- `copilot-instructions.md` には、プロジェクトで作っているもの、構成、全体的なコーディング規約など、プロジェクトレベルの情報を記載します。
- `*.instructions.md` には、単体テスト、Astro コンポーネント、データレイヤーなどのファイル形式や、特定の作業についての指示を記載します。
- 自然な言葉で、明確な指針とコードの良い例・悪い例を示します。

指示ファイルの作成方法は1つではありません。AI と同じように、実際に試し、プロジェクトやシナリオに合わせて調整してください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意してください。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまな作業向けの指示があります。
>
> Copilot に指示ファイルの作成を依頼することもできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` など、使用する画面ごとに方法が異なります。該当するレッスンで説明します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリには、中心となる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の **GitHub Actions Workflows** に関する指針も確認してください。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開きます。Astro、Drizzle データレイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、指示が適用されるファイルを指定する glob です。たとえば `**/*.test.ts` に一致する TypeScript のテストファイルに適用されます。
5. プロジェクトで単体テストを作成するときの指示を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾を確認します。`unit-tests.instructions.md` など他の指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。パスはリポジトリのルートではなく、指示ファイルを基準とします。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング規約が記載されていますが、コード内のドキュメントはまだ求められていません。次の手順で TSDoc コメントとファイルヘッダーの規則を追加します。

## リポジトリ全体に新しい基準を追加する

ここで行う編集は1つです。コード内に TSDoc コメントとファイルレベルのコメントヘッダーを追加する、プロジェクト全体の規則を設定します。後半の演習で cloud agent が issue に取り組むときに適用されます。

編集する前に、作業用ブランチを作成します。cloud agent は issue が対象とするブランチの指示を読みますが、変更を整理してコミットします。

1. Codespace のターミナルで新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` を開き、プロジェクトのコーディング規約を明確にします。

1. `.github/copilot-instructions.md` を開き直します。
2. 27行目付近にある **Code formatting requirements** セクションを探します。現在はコーディング規約がありますが、コード内のドキュメントに関する規則はありません。
3. 次の Markdown を既存の規則の直後に追加し、ファイルヘッダーと TSDoc コメントの追加を Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ全体の指示は `.github/copilot-instructions.md` に、言語やファイル形式、タスク固有の指示は `*.instructions.md` に記載できます。ここで追加するようなプロジェクト全体の規則には、リポジトリレベルのファイルが適しています。

## 指示の更新をコミット、プッシュ、マージする

cloud agent は issue の対象ブランチから指示ファイルを読み込みます。次の演習で issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。指示を読み込ませるには、変更を `main` にマージしておく必要があります。

1. ステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成してマージします。次の演習で割り当てる issue に取り組む際、cloud agent がこの指示を読み込みます。

> [!TIP]
> ワークショップで `main` に直接作業する場合は、ブランチ作成やマージを省略して `main` にコミットしてもかまいません。ここでは実際のプロジェクトで行う方法に合わせています。

## まとめと次のステップ

このプロジェクトの指示ファイルが Copilot に提供するコンテキストを確認し、`.github/copilot-instructions.md` にリポジトリ全体の基準を追加しました。cloud agent がこのハーネスの後半で作成するプルリクエストに、その基準が適用されます。

次は、[最初の issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタム指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示をより良く書くための5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
