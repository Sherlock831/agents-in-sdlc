---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: Custom agent →][next-lesson]

Agent mode を使ってフィルタリング機能を構築しました。プルリクエストを作成する前に、ブラウザーで実際に動作することを確認しましょう。自分でアプリを操作する代わりに **Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーで機能をテストしてもらいます。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) とは何か、MCP サーバーで Copilot に新しいツールを追加する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot にブラウザーでフィルタリング機能を手動テストするよう依頼します。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

Agent mode は、エディターの外部にアクセスできると、さらに強力になります。Model Context Protocol (MCP) を使うと、Copilot は外部のツールやサービスと通信できます。これは、agent が外部のツールやサービスとやり取りするための標準的な方法です。

![agent mode の内部構造と、コンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールとの連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)を使うと、AI agent は外部ツールやサービスと通信できます。MCP によって、AI agent は外部ツールやサービスとリアルタイムにやり取りし、resource を使って最新情報にアクセスしたり、tool を使ってユーザーに代わって操作を実行したりできます。

これらの tool や resource は、AI agent と外部のツールやサービスをつなぐ MCP サーバー経由で利用します。MCP サーバーは、既存の API や NPM package などのローカルツールとの通信を管理します。各 MCP サーバーによって、AI agent が利用できる tool や resource の種類が異なります。

よく使われる MCP サーバーの例を示します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。新しいリポジトリの作成、既存リポジトリの更新、Issue やプルリクエストの管理などを AI agent が実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。Web ページへの移動、フォームへの入力、ボタンの選択などを AI agent が実行できます。

さまざまな tool や resource にアクセスできる MCP サーバーがほかにも公開されています。GitHub は、エコシステム内での発見や貢献を促進するために [MCP registry](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーは他の依存関係と同じように扱ってください。利用する前にソースコードをよく確認し、公開元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の設定を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースが利用できる MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーは、すでに次のように設定されています。

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

`github` の設定はプロジェクトテンプレートに含まれています。そのため、前の演習で Copilot は Issue のバックログを読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使っているため、ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot がその tool を初めて使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が操作できるブラウザーを利用できます。これを使って機能をテストします。

1. `.vscode/mcp.json` で `github` の隣に `playwright` の設定を追加し、ファイルを次のようにします。

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

リモートの `github` サーバーとは異なり、`playwright` は**ローカル**サーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを指定すると、Playwright は表示ウィンドウなしでブラウザーを実行します。デスクトップ環境がない codespace では、この指定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトはエンドツーエンドテストでも Playwright を使用しているため、通常、Playwright が必要とするブラウザーはすでにインストールされています。Copilot がブラウザーがないと報告した場合は、`npx playwright install chromium` を実行するよう依頼して、もう一度試してください。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて**起動します。Copilot がサーバーの tool を初めて必要としたときに、VS Code がサーバーを起動し、信頼するかどうかを尋ねます。

1. `.vscode/mcp.json` の `playwright` 設定の上に **Start** が表示されます。選択してサーバーを起動し、設定が正しいことを確認します。
2. VS Code からサーバーを信頼するかどうか尋ねられた場合は、設定を確認して、サーバーの起動を許可します。

手動で起動する必要はありません。起動を省略しても、次の手順で Copilot が tool を初めて必要としたときに、VS Code がサーバーを起動して信頼を求めます。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]は、Copilot が操作できる実際のブラウザーを提供します。アプリを自分で操作して確認する代わりに、agent がページを開き、移動し、フィルターを適用して、その結果を読み取ってから内容をまとめます。会話を離れずに機能が期待どおりに動作することをすばやく確認できます。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティツリー][playwright-mcp-server]を使って動作します。そのため、agent は button、link、list item など、構造化されラベル付けされた要素を使って推論します。これは支援技術と同じ方法であり、簡単な機能確認をアクセシビリティの基本チェックとしても活用できます。

サーバーに接続し、アプリを実行した状態で、Copilot にフィルタリング機能を試してもらいます。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを使ってブラウザーを起動し、各手順を実行して結果を報告します。Issue に記載された受け入れ条件と結果を照らし合わせます。問題がありそうな場合は、質問を追加するか、プルリクエストを作成する前にコードの修正を依頼します。

> [!NOTE]
> このテストには、`http://localhost:4321` でアプリを実行している必要があります。dev server を停止した場合は、プロンプトを送る前にもう一度起動してください。Copilot が Playwright MCP サーバーを初めて使うとき、ブラウザーをダウンロードする必要がある場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行するよう依頼して、もう一度試してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認したので、チームがレビューできるようにプルリクエスト（PR）を作成しましょう。まず `filtering-vscode` ブランチを公開します。

1. Codespace で **Source Control** パネルに移動し、Copilot が加えた変更を確認します。
2. **+** アイコンを選択して変更を stage します。
3. **Sparkle** ボタンを使って commit メッセージを生成します。

   ![変更内容を表示する Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリに push します。

## プルリクエストを作成する

github.com や GitHub command-line interface (CLI) など、プルリクエストを作成する方法はいくつかあります。GitHub Copilot を使って作業しているので、PR の作成も依頼しましょう。関連する Issue を見つけ、Issue と関連付けた PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. Copilot が GitHub ツールを使用できるように、agent dropdown で **Agent** が選択されていることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、Copilot が情報を収集して操作を実行できるようにします。GitHub ツールを初めて使うときは、GitHub への認証を求められることがあります。画面の指示に従って認証します。
5. Copilot が Issue を検索し、該当するものを見つけて PR を作成する様子を確認します。
6. Copilot が生成したリンクを選択してプルリクエストを確認します。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) とは何か、MCP サーバーで Copilot に新しい tool を追加する仕組みを学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- 出荷前に、Playwright MCP サーバーを使ってブラウザーでフィルタリング機能を手動テストしました。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成しました。

MCP サーバーを使って機能を*テスト*しましたが、Copilot を拡張する方法は MCP だけではありません。次は、アクセシビリティレビューなど、対象を絞ったタスクを効率化する[custom agent を作成し][next-lesson]ましょう。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
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
