---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善と機能追加を目的に、GitHub Copilot と複数のタスクに取り組みました。コードへのドキュメント追加、デザインチームが改善できる関連ゲーム機能の実装、ハイコントラストとライトモードを含むアクセシビリティ機能を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って改善しましょう。

### シナリオ

生成 AI を利用しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チーム全体にレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した内容を確認します。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期に監視なしで作業するため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みは作業用のブランチにのみ許可されます。
- cloud agent は GitHub Actions 内に一時的な専用環境を作成して実行されます。
- GitHub Actions ワークフローは、人間が承認するまで実行されません。
- [外部リソースへのアクセスは既定で制限されます](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーへのアクセスも制限対象です。

## 生成されたドキュメントをレビューする

まず、コードにドキュメントを追加するために GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR) を、GitHub.com の標準的な PR 画面で確認します。

> [!NOTE]
> PR に GitHub Copilot がファイアウォールでブロックされたという警告が表示されることがあります。Copilot は MCP サーバーへの呼び出しなど、外部リソースへのアクセスが既定で制限されるため、これは想定された動作です。必要であれば、[Copilot cloud agent のファイアウォールを構成または無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルの PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が表示されます。作業が完了するまで待ちます。数分かかることがあるので、休憩したり、学んだことを振り返ったりしてください。

4. 準備ができたら **Files changed** タブを選択して、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc コメントやその他のドキュメントが追加されたコードを確認します。変更内容は環境によって異なります。演習 2 で確認したカスタム指示に基づく TSDoc コメント、TypeScript の規約、コメントヘッダーに注目してください。
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を探します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR の checks セクションにワークフローが登録されます。単一 Astro アプリのプロジェクトチェックが成功することを確認します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業するのは一方通行ではありません。チームメンバーに依頼するのと同じように、PR やコードのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するセッションを開始します。結果は確率的に変わるため、必ず特定の文章にする必要はありません。たとえば、次のような変更を依頼できます。

- 各コードファイルの先頭に、内容を簡潔に説明するコメントヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc コメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. Copilot にメンションするコメントを追加し、ドキュメントへの変更を依頼します。上記の例を使うか、コードベースに追加したい内容を提案します。
2. **View Session** を選択して、Copilot が作業する様子を確認します。更新用の新しいセッションが始まります。
3. **Back to pull request** を選択すると PR に戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、PR に新しいコミットが表示されます。
5. **Files changed** タブで変更をレビューします。

満足するまで改善を繰り返します。準備ができたら PR をドラフトからレビュー可能な状態に変更して、`main` ブランチにマージします。

![PR をレビュー可能な状態にする](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue を解決するため、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す表示を探します。
7. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. PR の checks セクションでワークフローが実行され、単一 Astro アプリのプロジェクトチェックが成功することを確認します。数分かかることがあります。
9. **オプション:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace でターミナルを開き、Copilot が作成したブランチ名（例: **copilot/fix-8**）に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

関連ゲーム機能ができました。前と同じように、PR の **Conversation** タブで Copilot にコメントを追加すれば、表示件数やコメントヘッダー、TSDoc コメントなどの変更を依頼して反復できます。後者はカスタム指示の更新前に割り当てたタスクであることに注意してください。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティカスタムエージェントが実装した機能を確認します。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 で追加を依頼したライトモードが含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が表示されます。作業が完了するまで数分待ちます。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注目します。
   - モード切り替え用 UI コンポーネント。
   - ユーザー設定を保存する local storage の利用。
   - ハイコントラストモードとライトモードの CSS またはスタイルの変更。
   - アクセシビリティ属性（ARIA ラベル、キーボード操作など）。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を探します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR の checks セクションでワークフローが実行され、単一 Astro アプリのプロジェクトチェックが成功することを確認します。完了まで数分かかることがあります。
10. **オプション:** Codespace でこのブランチに切り替えてアクセシビリティ機能を手動でテストします。ターミナルで Copilot が作成したブランチ名に置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動してブラウザーでハイコントラストとライトモードを試し、ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティカスタムエージェントによって、Copilot がアクセシビリティのベストプラクティスに沿って機能を実装できました。問題や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## オプションの演習 — 作業の委任を続ける

cloud agent は、実際のバックログ項目を任せて別の作業に移るときに効果的です。いくつか issue を作成して Copilot に割り当て、使い方に慣れてください。たとえば、次のような作業があります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。これらのレッスンでは、次の操作を行いました。

- このリポジトリに含まれるカスタム指示ファイルを確認し、cloud agent の出力に与える影響を確認しました。
- issue を Copilot cloud agent に割り当て、環境の準備、計画、非同期の作業を確認しました。
- アクセシビリティ用のカスタムエージェントを作成して使い、ハイコントラストとライトモードの切り替えを追加しました。
- Copilot agents ページをミッションコントロールとして使い、作業中にアクセシビリティセッションを監視して方向を示しました。
- cloud agent のプルリクエストをレビューし、`@copilot` に変更を依頼してワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスは完了です。学んだ内容をさらに実践するには、次のハーネスを確認してください。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE で Copilot Chat の agent mode と MCP 統合を体験します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェントスキル、カスタム QA エージェント、Playwright MCP、Agent Merge、任意の `/delegate` による cloud agent への連携を使い、ターミナルから2つの変更をレビューしてリリースします。

自分のリポジトリでは、次の作業を試してください。

- リファクタリングやテスト範囲の拡大を cloud agent に割り当てる。
- 別の分野向けのカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
