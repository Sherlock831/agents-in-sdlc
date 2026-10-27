---
title: "演習 2 - Copilot Agent mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Custom instruction][previous-lesson] · [次のレッスン: Agent mode を Playwright MCP でテストする →][next-lesson]

アプリケーションの簡単な更新でも、通常は複数のファイルを変更し、テストを実行する必要があります。開発者は、関連するファイルを探し、変更を加え、テストを実行し、問題をデバッグし、更新し忘れたファイルを見つけて、さらに変更するという作業を繰り返します。

このようなときに Copilot Agent mode が役立ちます。

Copilot Agent mode は IDE 内で、より自律的に作業するように設計されています。開発者と同じように、まず既存のプロジェクト構造を調べ、必要なファイルを更新し、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent mode を使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途向けに作られています。Agent mode は IDE 内で作業し、短いフィードバックサイクルでやり取りできます。Cloud agent は issue とプルリクエストを通じてチームメンバーのように非同期で作業します。

この演習では、次のことを学びます。

- Copilot Agent mode がプロジェクトを調べ、関連ファイルを特定し、複数のファイルを連携して変更する方法
- GitHub Copilot Agent mode が UI とデータレイヤーにまたがる機能を実装する方法
- コードベースにマージする前に、Copilot Agent mode が生成した変更とテストをレビューする方法

## シナリオ

ゲームの一覧が増えてきたため、カテゴリと publisher で絞り込めるようにします。前の演習で publishers helper を追加しました。次は、Copilot Agent mode を使って残りのデータレイヤー、UI、テストの作業を完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認し、現在の機能を理解しましょう。

この Web サイトは、開発者向けのテーマを使ったボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示します。サイトは Astro アプリ 1 つで構成され、ビルド時に静的 HTML を生成します。ページは frontmatter 内で Drizzle ORM を使ってローカルの SQLite データベースを直接参照します。独立したバックエンド API やクライアントサイドの UI framework はありません。再利用可能なデータアクセス helper は `src/lib/` に配置し、インタラクティブな機能は標準の DOM API を使う小規模な Astro `<script>` で追加します。

### Web サイトを起動する

`npm run dev` タスクを使うと、Astro の dev server を起動できます。GitHub Codespace で次の手順を実行します。

1. 前の演習で使った Codespace に戻ります。`main` ブランチにいるはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルウィンドウを開きます。
3. フィルタリング作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro dev server の準備が完了すると、次のような URL を含むメッセージが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くかどうかを尋ねるダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルで Astro server のアドレス `http://localhost:4321` を選択します。Mac では <kbd>Command</kbd> キー、Windows/Linux では <kbd>Ctrl</kbd> キーを押しながら選択してください。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは Web server をホストする codespace へのプライベートなトンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。主な機能は次のとおりです。

- **ホームページ**: タイトル、画像、説明を含むボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、publisher、カテゴリなどの情報を含む詳細ページが表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は手順で説明します。

Web サイトはすでに動作していますが、さらに機能を追加します。まずバックログを確認しましょう。テンプレートからプロジェクトを作成したときに Issue のバックログも自動的に作成されています。GitHub Copilot に内容を表示するよう依頼します。

1. Chat ビューの agent dropdown で **Agent** を選択します。Agent は自律的に計画を立て、複数のファイルに変更を加え、ターミナルコマンドを実行し、ツールを呼び出します。

   ![Chat ビューの agent picker を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨するものではありません。ワークショップの作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案の一貫性を高め、スムーズな体験にすることを目的としています。ただし、LLM の動作は確率的であるため、ワークショップの例とは異なる提案が表示されることがあります。これは正常な動作です。

> [!NOTE]
> LLM の動作は確率的なため、Copilot は別の MCP コマンドを使うことがあります。それでもタスクを完了できるはずです。

3. 次のプロンプトを Copilot に送信して、Issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. すべての Issue を一覧表示するコマンドを実行するため、**Continue** を選択します。
5. Copilot が生成した Issue の一覧を確認します。

Copilot がユーザーに最も役立つと思われる項目を優先順位付けしたことを確認できます。

## Instruction file を確認する

Copilot にコードを生成させる前に、作業のコンテキストを渡すために使う instruction file を確認しましょう。Web サイトに機能を追加する際の指針を含む [user interface (UI)][ui-instructions] file を使います。

1. Codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加のアプローチに関する全体的なガイダンスを確認します。次の内容が含まれています。
   - アーキテクチャの概要
   - component の設計、テスト容易性、アクセシビリティの原則
   - Astro、Tailwind CSS、Drizzle などのファイル種別に特化した instruction file へのリンク

> [!TIP]
> Instruction file から、別の instruction file とプロジェクト内のファイルの両方を参照できます。パスは instruction file がある場所からの相対パスです。これにより、再利用しやすい小さなファイルに分割したり、例やテンプレートを示したりできます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、少なくとも 3 つの箇所を更新する必要があります。

- データレイヤー（`src/lib/`）の残りのフィルタリングロジックを追加または調整する
- フィルタリングの動作をテストするためのテストを追加または更新する
- ゲーム一覧ページにフィルタリング UI を追加する

さらに、コードベースにマージする前にテストを実行し、成功させる必要があります。Copilot Agent mode にこれらの作業を任せましょう。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択し、instruction file を追加します。

   ![UI instruction file を選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューの agent dropdown で **Agent** が選択されていることを確認します。

   ![Chat ビューの agent picker を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルに **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログにある関連 Issue の要件に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、関連するファイルを見つける様子を確認します。データレイヤーの helper、UI、テストを特定してからファイルを変更し、テストを実行します。

   ![Copilot がプロジェクトのファイルを調べている様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストなど、いくつかのタスクを実行します。タスクやコードベースの複雑さによって、数分かかることがあります。作業中、コードエディターに **Keep** と **Undo** ボタンが表示される場合があります。Copilot の作業が完了すると、すべての変更に対して **Keep** または **Undo** を選べるため、作業中に選択する必要はありません。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![テストを実行してよいか確認する Copilot Chat のダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はコード生成とテストを繰り返し、エラーが見つからなくなるまで修正します。

   ![Copilot Agent mode とのチャットセッション完了画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新しい機能を確認します。
11. 動作を確認してコードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot Agent mode を使って Tailspin Toys Web サイトに機能を追加しました。次のことを学びました。

- GitHub Copilot Agent mode が UI とデータレイヤーにまたがる機能を実装する方法
- Copilot Agent mode がプロジェクトを調べ、関連ファイルを特定し、複数のファイルを連携して変更する方法
- コードベースにマージする前に、Copilot Agent mode が生成した変更とテストをレビューする方法

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加の演習 — ページ分割を実装する

ゲームの一覧が増えるにつれて、ページ分割が必要になります。この演習で学んだことを使い、Copilot にページ分割を追加するよう依頼してみましょう。次の点を検討してください。

- 既存の instruction file を使うなど、現在のベストプラクティスに従います。
- ユーザーがページあたりの表示件数を選べるようにするか、値を固定するかなど、ページ分割の方法を検討します。
- 希望どおりの実装に必要なガイダンスを Copilot に渡すプロンプトを作成します。
- GitHub Copilot にコンテキストを追加して変更を依頼し、何度かやり取りする必要があるかもしれません。Copilot と作業する場合は通常の流れです。

## リソース

- [Copilot の ask、edit、agent mode: 機能と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
