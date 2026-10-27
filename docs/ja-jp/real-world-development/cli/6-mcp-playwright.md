---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーでフィルタリング機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くこと以外にも、データや外部サービスとの連携、Copilot が利用する自動化機能の追加などが必要です。MCP サーバーを使うと、Copilot CLI に組み込まれている機能以外のツールやサービスも利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように利用するかを理解します。
- まだ利用できない場合は、Playwright MCP サーバーを追加します。
- agent にブラウザーを操作させ、フィルタリング機能を試します。

## シナリオ

unit test とエンドツーエンドテストは重要ですが、UI の更新を検証するには、実際に UI を操作する必要があります。Copilot に開発中の Web サイトをユーザーと同じように操作させることで、変更が期待どおりに動作する確度を高め、作業をさらに自動化します。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post]を使うと、AI agent は外部ツールやサービスと通信できます。MCP によって、AI agent は外部ツールやサービスとリアルタイムにやり取りし、最新情報にアクセスしたり、ユーザーに代わって操作を実行したりできます。

これらの tool や resource は、AI agent と外部のツールやサービスをつなぐ MCP サーバー経由で利用します。各 MCP サーバーによって、AI agent が利用できる tool や resource の種類が異なります。

よく使われる MCP サーバーの例を示します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プルリクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

さまざまな MCP サーバーが利用できます。GitHub は、エコシステム内での発見や貢献を促進するために [MCP registry][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーは、プロジェクト内の他の依存関係と同じように扱ってください。利用する前にソースコードを確認し、公開元を検証して、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、Copilot がユーザーと同じように Web サイトを操作できるように追加します。

> [!NOTE]
> Playwright MCP がすでに Copilot CLI に設定されている場合は、設定手順を省略して次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバー追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類として **STDIO** を選択し、<kbd>Enter</kbd> を押してから <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを終了します。

## Playwright を使って Copilot に機能を試してもらう

前の手順では、機能が期待どおりに動作することを手動で確認しました。次は、Copilot CLI が利用できる Playwright MCP サーバーを使って同じ確認を行いましょう。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常は Copilot が自動的に MCP サーバーを使うため、明示的に指定する必要はありません。ただし、使用する tool がわかっている場合は、適切な方向を示すと結果の一貫性が高まり、token の節約にもなります。

2. Copilot がブラウザー内で実行する各手順を確認し、機能が動作するか確認します。
3. Copilot の報告を読み、すべて期待どおりに動作することを確認します。

Copilot は server を起動し、Playwright で Web サイトを操作し、作業後に server を停止して結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を試しました。次のことを行いました。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように利用するかを学びました。
- Playwright MCP サーバーがまだ利用できない場合に追加しました。
- agent にブラウザーを操作させ、フィルタリング機能を試しました。

次は、skill とブラウザーツールを専門的な役割としてまとめる [QA custom agent を作成し][next-lesson]ます。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
