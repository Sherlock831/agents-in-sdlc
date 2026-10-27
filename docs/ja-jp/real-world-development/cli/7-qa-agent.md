---
title: "レッスン 7 - QA agent を作成して使う"
description: "Issue 要件、quality-checks skill、Playwright MCP を組み合わせる QA custom agent を作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` skill を使って自動チェックを実行し、Playwright MCP を使ってブラウザーでフィルタリング機能を確認しました。次は、それらを明確な QA プロセスを持つ custom agent にまとめます。

このレッスンでは、次のことを行います。

- Custom agent と instruction、skill、MCP tool の連携を確認します。
- 再利用可能な品質保証プロファイルを作成して調べます。
- QA agent を選択し、フィルタリング Issue に照らして結果を確認します。

## シナリオ

Tailspin Toys は、プルリクエスト（PR）を作成する前に、要件、コード品質、自動チェック、テストカバレッジ、ブラウザーでの動作を一貫してレビューしたいと考えています。Custom agent を使うと QA プロセスを調整し、再利用可能なレポートを作成できます。

## Custom agent とは

Custom agent は、Markdown プロファイルで定義する Copilot の特化バージョンです。プロファイルでは、agent の目的、指示、利用できる tool を指定します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの instruction はチームの標準を説明し、`quality-checks` skill は繰り返し実行できるチェックをまとめ、Playwright MCP はブラウザーの tool を提供します。QA プロファイルは、それらを使って要件を評価し、結果を報告する方法を Copilot に伝えます。既存の機能を置き換えるものではなく、新しい会話も必要ありません。

## QA プロファイルを作成する

機能の PR を作成する前に、再利用可能な QA プロファイルを Copilot に作成してもらいます。プロファイルには、QA が実行するチェックと守るべき境界の両方を定義します。

1. Codespace に戻り、フィルタリングの会話が Interactive mode であることを確認します。
2. default agent に新しい custom agent を作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しい agent を使う前に、Copilot が意図した QA ワークフローと境界を反映したことを確認します。検証だけを行いたいときに、agent が不完全または広すぎる内容で機能を変更するのを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須です。`name` は任意ですが、含めると agent の表示名がわかりやすくなります。
3. プロファイルの指示を読み、QA が要件を起点とし、リポジトリの instruction に従い、`quality-checks` skill を実行して、Playwright MCP を使うことを確認します。
4. QA が根拠を示し、実装コードを変更する前に確認を求め、変更を commit したりプルリクエストを作成したりしないことを確認します。
5. これらの役割や制約が不足している場合は、続行する前に default agent にプロファイルの修正を依頼します。

## Issue を基に QA を実行する

Copilot CLI は会話の開始時にプロジェクト agent を読み込みます。プロファイルを作成した後、同じフィルタリングの会話を再開し、QA を選択します。Issue と計画の決定事項をそのまま利用できます。

1. 次のプロンプトで agent を有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成したばかりの agent は、利用可能な agent の一覧に表示されないことがあります。上のコマンドを実行すると有効になります。

2. QA agent に機能のレビューを依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA agent が作業を開始します。
4. 作業が完了したら、レポートを確認します。

## まとめと次のステップ

再利用可能な専門的役割をワークフローに加え、結果をレビューしました。このレッスンでは、次のことを行いました。

- Custom agent と instruction、skill、MCP tool の連携を確認しました。
- 再利用可能な品質保証プロファイルを作成して調べました。
- QA agent を選択し、フィルタリング Issue に照らして結果を確認しました。

実装、skill の更新、QA プロファイル、テスト、検証レポートの準備ができました。次は、それらを[機能 PR にまとめ、Agent Merge を使い][next-lesson]ます。

## リソース

- [Copilot CLI 用の custom agent を作成する][create-agents]
- [Custom agent の設定][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
