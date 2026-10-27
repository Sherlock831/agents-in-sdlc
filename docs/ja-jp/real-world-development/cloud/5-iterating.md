---
title: "演習 5: GitHub Copilot の作業を改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善と機能追加を目的に、GitHub Copilot といくつかのタスクに取り組みました。コードへのドキュメント追加、デザインチームが試せる関連ゲーム機能の作成、ハイコントラストモードとライトモードを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを提供して作業を改善しましょう。その後、チームのほかのメンバーにもレビューを有効にします。

### シナリオ

生成 AI を導入しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って進める必要があります。チーム全体のレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能を実装するための GitHub Copilot の提案を確認しましょう。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期かつ監督者なしでタスクを実行するため、安全性を保つためのセキュリティ制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みは作業用ブランチに限られます。
- Cloud agent は GitHub Actions 内で動作し、作業用の独立した一時環境を作成します。
- GitHub Actions のワークフローを実行するには、人間による承認が必要です。
- MCP サーバーを含む[外部リソースへのアクセスは、既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、コードにドキュメントを追加するために GitHub Copilot cloud agent が作成した最初のプルリクエスト（PR）を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR に、GitHub Copilot がファイアウォールでブロックされたという警告が表示されることがあります。Copilot の外部リソース（外部 MCP サーバーへの呼び出しなど）へのアクセスは既定で制限されているため、これは**想定どおりの動作**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** などのタイトルのプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストには **[WIP]** が付いています。その場合は Copilot の作業が完了するまで待ちます。数分かかることがあるので、休憩したり、ここまでに学んだことを振り返ったりしてください。

4. 準備ができたら **Files changed** タブを選択し、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc の doc comment など、更新されたコードとドキュメントを確認します。変更内容は状況によって異なります。レビューでは、演習 2 の最初に確認した custom instruction file による TSDoc の doc comment、TypeScript 規約、コメントヘッダーに注目してください。
6. 更新内容をレビューして問題がなければ、**Conversation** タブに戻って下にスクロールします。
7. ワークフローが承認待ちであることを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストの checks セクションにワークフローが登録されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業する方法は、提案を受け取るだけではありません。チームのほかのメンバーと同じように、プルリクエストのコメントやコードのインラインコメントで Copilot にメンションできます。Copilot はコメントを認識し、対応するための新しいセッションを開始します。結果は非決定的なため、必ず同じ結果になる具体的な依頼文は示せません。たとえば、次のような変更を依頼できます。

- 各コードファイルの先頭に、ファイルの概要を示すコメントヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc の doc comment を追加する。
- Astro アプリの構造を説明する README を作成する。

1. 上記の例またはコードベースに必要な別の提案を使って、生成されたドキュメントに対する変更を **@copilot** にメンションしてコメントします。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot は更新のために新しいセッションを開始します。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

    ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、プルリクエストに新しい commit が追加されます。
5. **Files changed** タブを選択し、変更をレビューします。

納得できるまで改善を続けてください。完了したら、PR をドラフトからレビュー可能な状態に変更して、main ブランチにマージできます。

![PR をレビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

次は、ゲーム詳細ページに関連ゲームを表示する Issue を解決するために Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新をレビューして問題がなければ、**Conversation** タブに戻って下にスクロールします。
6. ワークフローが承認待ちであることを示す表示を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. プルリクエストの checks セクションにワークフローが登録されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します（`<branch-name>` は Copilot が作成したブランチ名（例: **copilot/fix-8**）に置き換えます）。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前の作業と同じように、Copilot cloud agent に更新を依頼して反復できます。たとえば、表示する関連ゲームの数を変更したり、コメントヘッダーと TSDoc の doc comment を追加したりできます（この Issue は custom instruction を更新する**前**に割り当てました）。**Conversation** タブに新しいコメントを追加すると、Copilot が内容を確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用の custom agent を使って実装した機能をレビューします。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 の mission control で追加を依頼したライトモードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストには **[WIP]** が付いています。その場合は、Copilot の作業が完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注目します。
   - 表示モードを切り替える UI component
   - ユーザー設定を保持する local storage の使用
   - ハイコントラストモードとライトモードの CSS またはスタイルの変更
   - アクセシビリティ属性（ARIA ラベル、キーボード操作など）
   - 表示モードの切り替えを管理する JavaScript/TypeScript コード
6. 更新内容をレビューして問題がなければ、**Conversation** タブに戻って下にスクロールします。
7. ワークフローが承認待ちであることを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストの checks セクションにワークフローが登録されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動し、ターミナルで次のコマンドを実行します（`<branch-name>` は Copilot が作成したブランチ名に置き換えます）。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次にアプリケーションを起動し、ブラウザーでハイコントラストモードとライトモードの切り替えをテストします。ページを再読み込みしても設定が保持されることを確認してください。

アクセシビリティ custom agent が、アクセシビリティのベストプラクティスに沿って Copilot の実装を導いたことを確認できます。アクセシビリティ上の問題や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — さらに作業を委任する

Cloud agent は、実際のバックログ項目を任せて別の作業に集中するときに役立ちます。いくつかの Issue をリポジトリに登録して Copilot に割り当て、この使い方に慣れましょう。次のような案があります。

- ゲーム詳細ページに支援者向けの関心表明フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセス helper に入力検証とエラー処理を追加する。

## まとめ

Cloud agent のハーネスを完了しました。これらのレッスンを通じて、次のことを行いました。

- **このリポジトリに用意された custom instruction file を確認**し、cloud agent の出力に反映される様子を確認しました。
- **Copilot cloud agent に Issue を割り当て**、環境の準備、計画、非同期の作業を確認しました。
- **アクセシビリティ用の custom agent を作成して使用**し、ハイコントラストモードとライトモードの切り替えを追加しました。
- **Copilot agents ページを mission control として使い**、アクセシビリティセッションを監視して途中で誘導しました。
- **cloud agent のプルリクエストをレビューして改善**し、**@copilot** に変更を依頼してワークフローを承認しました。

## 振り返りと次のステップ

Cloud agent のハーネスを完了しました。学んだ内容をさらに実践するには、次のハーネスも試してください。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat の agent mode と MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot mode、agent skill、custom QA agent、Playwright MCP、Agent Merge、任意の `/delegate` による cloud agent への連携を使って、ターミナルからレビュー済みの変更を 2 つ届けます。

自分のリポジトリでは、次のような追加作業も試せます。

- リファクタリングやテストカバレッジの Issue を cloud agent に割り当てます。
- 別の分野向けの custom agent を作成します。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

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
