---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント作成の基準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法で作業してほしい場合は、その指針を Copilot が参照できるようにします。[指示ファイル][instruction-files]では、求めるコードだけでなく、その構成方法も伝えられます。フィルター機能を実装したので、Copilot が参照した指示を確認し、ドキュメント作成の基準を追加して、コードに適用します。

このレッスンでは、次の操作を行います。

- リポジトリの指示とパスに応じた指示ファイルがエージェントに適用される方法を確認します。
- コーディング規約を守るために指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発のガイドラインと要件があります。たとえば、次のとおりです。

- コメントでは、コードを言い換えるのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記述します。`db` 引数を注入する場合は、その引数についても記述します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明し、関連コードが変更されたらコメントも最新に保ちます。
- 既存の書式設定と lint のガイダンスを維持します。

指示ファイルを使って、Copilot がこれらの開発方針に沿って作業できるようにします。

## 指示ファイル

カスタム指示は、コーディングスタイルや要件を Copilot に伝えるコンテキストとして使えます。ライブラリや規約、コードコメントの書き方などを指定し、より適切な提案やコード生成につなげられます。リポジトリ全体または特定のファイル形式や作業に対して指示を作成できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に渡す単一の指示ファイルです。ほとんどのリクエストに関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル形式、作業向けのガイドラインを提供します。

> [!NOTE]
> その他の指示形式とサポート状況は環境によって異なります。特定の形式に依存する前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示を確認する

スターター プロジェクトには、すでに複数の指示ファイルがあります。変更の影響を確認するため、まず既存の内容を調べます。

1. Codespace に戻ります。
2. Codespaces エディターで（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング指針を確認します。この指示は、このリポジトリで Copilot を使うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドで、指示が適用されるファイルを指定する glob を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルやプロジェクト内ファイルへのリンクを確認します。これにより大きな指示を小さな再利用可能ファイルに分け、参考例を Copilot に示せます。

## チームの方針に合わせて指示ファイルを更新する

既存のファイルは出発点として十分ですが、TypeScript に新しく追加するコードへの TSDoc コメントが求められていません。`copilot-instructions.md` を変更します。

1. `.github/copilot-instructions.md` の **Code formatting guidance** セクション（35行目付近）を探します。
2. 次の項目を最後に追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新した指示を適用する

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。指示を保存したら、フィルター機能の会話を終了して再開し、機能のコンテキストを保ったまま更新された指示を読み込ませます。

1. フィルター機能の会話が開いている場合は、`/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチでリポジトリのルートから、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合はフィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 更新した指示を適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力して変更された TypeScript ファイルを確認します。新しい TSDoc コメントが正確にコードを説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルを読み込む方法を確認し、新しい基準をフィルター機能に適用しました。具体的には、次の操作を行いました。

- リポジトリの指示とパスに応じた指示ファイルがエージェントに適用される方法を確認しました。
- コーディング規約を守るために指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、再利用可能な `quality-checks` スキルを[カスタマイズして実行][next-lesson]し、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI のカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
