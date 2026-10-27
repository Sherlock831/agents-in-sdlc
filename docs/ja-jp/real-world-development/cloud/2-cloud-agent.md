---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: Custom instruction][previous-lesson] |
|:--|

技術的負債に悩まされていない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加といったタスクを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を任せて開発プロセスを加速し、開発者がより大きな課題に集中できるようになります。

Copilot cloud agent を使って、次のことを確認します。

- コードを生成する環境のカスタマイズ
- 操作を安全に行う方法
- Issue の範囲を明確に定めることの重要性
- Copilot への Issue の割り当て

## シナリオ

Tailspin Toys には対処したい技術的負債があります。最初のサイトを構築した委託先がドキュメントを適切に整備しなかったため、コードにはドキュメントがほとんどありません。まず、アプリケーションのすべての export された関数に TSDoc の doc comment を追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属するほかのゲームを表示し、ユーザーがさらにゲームを探せるようにします。洗練された実装はまだ必要ありません。ユーザー体験の受け入れテストに使えるものを求めています。この作業は現在ブロッカーですが、優先度がより高い別の Issue があります。

どちらも、優先度が下がりやすいタスクの例です。Copilot cloud agent に割り当てると、非同期に作業を任せられます。その間、別のタスクを進め、後から Copilot の作業をレビューできます。

## GitHub Copilot cloud agent の紹介

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。[GitHub issue を Copilot に割り当てる][assign-issue]などの方法があります。割り当てると、Copilot は進行状況を追跡するドラフトプルリクエストを作成し、環境を準備して、タスクに取り組み始めます。実行中または完了後にセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 適切な範囲の指示が重要

GitHub Copilot に魔法はありません。考える手間を省けるわけではありません。一見簡単な操作でも、詳しく調べると複雑なことがあります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペアプログラマーとして扱い、段階的に進め、学び、試し、必要に応じて調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリの custom instruction

前の演習では、custom instruction と、それが Copilot を導く仕組みを確認しました。custom instruction の演習をすでに終えている場合は、`.github/copilot-instructions.md` が機能することを確認できているはずです。まだの場合は、ここで少し確認しましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリに用意されている instruction file を読み取り専用で確認すると、後からその効果を見つけやすくなります。

リポジトリの GitHub Web UI、または起動済みの codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript 規約と TSDoc の doc comment に関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` frontmatter を確認します。この instruction は `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てるとき、作成されたプルリクエストに TSDoc の doc comment、TypeScript 規約、コメントヘッダーが反映されているかを確認します。これらのルールは instruction file によるものです。後の演習で PR をレビューするときにも取り上げます。

## Copilot cloud agent の開発環境を準備する

誰が作業する場合でも、コードの作成には通常、特定の環境やスクリプトの実行などの準備が必要です。ソフトウェアエンジニアのように作業する Copilot にタスクを割り当てる場合も同じです。

Cloud agent は作業時に [GitHub Actions][github-actions]を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係を使えるよう環境をカスタマイズできます。ラボを円滑に進め、学習に集中できるよう、あらかじめ設定してあります。Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、単一 Astro アプリ用にマイグレーションおよび seed 済みの SQLite データベースを利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *手順*に必要な範囲で最小限の権限を設定します。Copilot の操作には専用の token が付与されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリを clone する場合は、`contents: read` 権限が必要です。セットアップ手順で clone しない場合は、手順の完了後に Copilot が自動的に clone します。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js（アプリ全体は Astro + Drizzle/libSQL で構成されています）
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

      # build とテストでデータを使えるよう、ローカルの SQLite データベースを migrate して seed します。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同じ構成ですが、次の点に注目してください。

- `copilot-setup-steps` という単一の job があります。Copilot がプルリクエストの作業を始める前に、GitHub Actions でこの job が実行されます。
- `workflow_dispatch` トリガーが設定されています。リポジトリの **Actions** タブからワークフローを手動実行できるため、Copilot が実行するのを待たずに、正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、情報が古かったり、そもそもなかったりするプロジェクトは少なくありません。放置されやすい技術的負債であり、生産性を下げ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメント作成を得意としているため、cloud agent に割り当てるのに適した Issue です。バックグラウンドで必要なドキュメントを生成します。後の演習で成果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次のテキストを入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

    ![Issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト（PR）を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。一覧に PR が表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後に、Copilot が TODO リストを作成します。

> [!NOTE]
> Copilot の TODO リストが PR に表示されるまで数分かかることがあります。Copilot は、前に説明したワークフローの実行、プロジェクトの分析、問題に取り組むための最適な方法の決定を行っています。

13. TODO リストを確認し、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

    ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するため、ブラウザーを更新する必要がある場合があります。

16. ライブセッションをスクロールし、Copilot がどのように問題を解決しているかを確認します。コードを調べて状態を理解し、考えながら適切な計画を決め、コードを作成していく様子を確認できます。

完了まで数分かかることがあります。Copilot cloud agent の主な目的の 1 つは、非同期で作業し、別のタスクに集中できるようにすることです。この機能を使って、もう 1 つのタスクも Copilot cloud agent に割り当ててから、アプリケーションの機能追加に取り組みましょう。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の 1 つは、作業を分担できることです。デザインチーム向けに関連ゲームのセクションを追加する作業にはそれほど時間がかからないかもしれませんが、その時間をほかの作業に使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次のテキストを入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

    ![Issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Issue の最初のコメントに 👀 が表示され、Copilot が作業中であることがわかります。

![目の絵文字で作業中であることを示す Copilot の Issue コメント](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェアエンジニアと同じように動作するため、積極的に監視する必要はありません。完了後にレビューしましょう。ここからは custom agent の作成と使用を確認します。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents]を確認しました。Cloud agent に Issue を割り当てると、非同期に作業を実行できます。技術的負債への対処、新機能の作成、別のフレームワークへのコード移行などを依頼できます。

次のことを確認しました。

- コードを生成する環境のカスタマイズ
- 操作を安全に行う方法
- Issue の範囲を明確に定めることの重要性
- Copilot への Issue の割り当て

Cloud agent がバックグラウンドで作業している間に、custom agent の作成と使用を確認しましょう。[Copilot cloud agent は MCP サーバーも使用でき][cloud-agent-mcp]、前のモジュールで確認した custom instruction も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: Custom agent →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
