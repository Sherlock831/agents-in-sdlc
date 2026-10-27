---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、基本操作を確認して、登録済みのフィルタリング Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作する強力な agent 型コーディングアシスタントです。コードベースを調べ、コードを生成し、コマンドを実行し、外部 tool に接続できます。コマンドラインから作業できるため、タスクを任せたり、変更を依頼したりしながら、作業の流れを保てます。まずは、使い慣れた tool を使って Copilot CLI をインストールしましょう。

このレッスンでは、次のことを行います。

- npm を使って GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼し、簡単な会話を試します。
- 組み込みの GitHub MCP サーバーを使って、フィルタリングの Issue を見つけます。

## シナリオ

チームでは、増え続けるバックログに対応するために AI agent を使い始めています。Copilot CLI は、多くの開発者が日常的に作業するターミナルでその機能を使えるようにします。このレッスンでは、インストールと認証を済ませ、ワークショップの残りで利用できる状態にします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew でインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、ここでは npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされており、バージョン要件を満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

初回起動時に、Copilot CLI は GitHub アカウントでの認証を求めます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 求められた場合は、device code の手順に従って認証し、Copilot CLI を承認します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されたパスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択して信頼します。

> [!NOTE]
> Codespace では、GitHub セッションを通じてすでに認証されている場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めて問題ありません。

## 使い方を確認する

通常の shell prompt で入力したコマンドは、Codespace で直接実行されます。Copilot CLI の起動後は、自然言語を agent に送信し、slash command で会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力してインストール済みのバージョンで利用できる command を確認し、<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. 次の質問をして、すべてが動作することを確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. Copilot が回答の前にリポジトリを調べることを確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用できることを確認します。
6. 次のプロンプトを使って、フィルタリングの Issue を Copilot に検索させます。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. 表示された URL を開いて Issue を読みます。最初の変更を行った後、この Issue を使います。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトされているブランチ上で作業し、自動的に worktree を作成しません。変更を始める前に、機能ブランチを作成します。

## ワークショップ用のショートカットを使う

通常、Copilot CLI は許可済みの範囲外で tool を使う前に確認を求めます。このワークショップでは `--yolo` を付けて再起動します。Codespace 内でその確認を省略できるよう承認済みのショートカットを使い、演習に集中します。

> [!CAUTION]
> `--yolo` は、完全な自動権限（`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`）を有効にします。Codespace や VM などの隔離環境でのみ使い、日常の開発で既定値として alias に設定しないでください。詳細は [Allowing and denying tool use][allow-all-warning] を参照してください。

`--enable-all-github-mcp-tools` を指定すると、後のレッスンで Issue やプルリクエストに使う GitHub MCP の読み取り / 書き込み tool が有効になります。Codespace からローカルコンピューターへのアクセスは制限されていますが、認証された GitHub リソースには引き続きアクセスできます。変更を公開またはマージする前にレビューしてください。

1. `/exit` を入力して Copilot CLI を終了します。
2. リポジトリルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについて簡単な質問をし、会話が動作することを確認してから `/exit` を入力します。

Copilot は会話を自動的に保存します。後で instruction を変更したり agent を追加したりした後、`copilot --resume` を使って同じ機能の会話とブランチに戻ります。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- npm を使って GitHub Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- ワークショップのリポジトリを信頼して、簡単な会話を試しました。
- 組み込みの GitHub MCP サーバーを使って、フィルタリングの Issue を見つけました。

次は、[最初の小さな変更を開始][next-lesson]し、Copilot CLI でゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
