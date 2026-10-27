---
title: "レッスン 2 - 星評価の追加: すばやい改善"
description: "Copilot CLI でゲームカードを小さく変更し、転送されたブラウザーで確認して、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールし、会話を試しました。次は、プロジェクトに初めて変更を加えます。変更は小さくします。ゲームのデータには星評価がありますが、ホームページのゲームカードにはまだ表示されていません。agent に評価を表示するよう依頼し、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次のことを行います。

- 機能ブランチで Copilot の会話を開始します。
- プロジェクトに小さな変更を加えるよう agent に依頼します。
- `/diff` で変更をレビューします。
- port を転送したブラウザーでアプリを実行し、変更を確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。しかし、ホームページのゲームカードにはタイトル、カテゴリ、publisher、説明しか表示されていません。最初の作業として、agent に既存の評価を各カードに表示してもらいます。小さく独立した変更なので、最初のセッションに適しています。

## 会話の構成

**会話**では、Copilot CLI を使ってタスクに取り組みます。Copilot app とは異なり、通常の CLI 会話では、専用の worktree を作成せず、ターミナルで現在チェックアウトされているリポジトリと Git ブランチを使います。保存した会話は後で再開できます。ファイルとブランチは、通常の Git の状態としてディスク上に残ります。

会話には、プロンプトと agent の応答、ファイルの調査・編集時の tool の操作、`/diff` で確認できる変更が表示されます。

## 会話を開始して変更を依頼する

新しい会話を始めて、機能の実装を開始します。

1. Codespace に戻ります。
2. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
3. まだ Copilot を起動していない場合は、次のコマンドを実行します。

   ```bash
   copilot --yolo
   ```

4. slash command `/new` を実行して <kbd>Enter</kbd> を押し、新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを確認し、ゲーム詳細の表示に使うファイルを探して必要なコードを作成します。Copilot CLI を使って新しい機能を追加できました。

## 差分をレビューする

小さな変更であっても、AI が生成した変更はマージする前にレビューします。Copilot CLI 内で変更を確認しましょう。

1. `/diff` を入力し、変更されたすべてのファイルを確認します。
2. ゲームカードに、評価がある場合は数値、`starRating` が `null` の場合は `No rating yet` が表示されることを確認します。
3. どちらの状態もテストで確認されていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を終了します。

> [!NOTE]
> すべての生成 AI ツールと同様に、Copilot の動作は決定論的ではありません。コードは例と異なる場合があります。特定の実装を期待するのではなく、動作を確認してください。

## 変更を確認する

コードを読むだけで動作すると判断するのではなく、Copilot に Web サイトを起動してもらい、Codespaces の port 転送で開いたブラウザーで UI を確認します。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces で port `4321` が利用可能になったら **Open in Browser** を選択します。
3. ゲームカードに 5 段階評価が表示されることを確認します。
4. Copilot に戻り、起動した server を停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプルリクエストを作成してマージする

機能を作成できました。次は、プロジェクトにコードをマージするためにプルリクエスト（PR）を作成します。

1. default agent に変更の commit を依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は PR の作成時に既存の commit を push し、PR の URL を表示します。
3. Copilot CLI が表示した URL を、Mac では <kbd>Command</kbd> キー、Windows/Linux では <kbd>Ctrl</kbd> キーを押しながら選択して PR を開きます。
4. 変更されたファイルとチェックをレビューします。
5. 準備ができたら **Merge pull request** を選択し、マージを確定します。
6. Codespace に戻り、`/exit` を入力して Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

GitHub Copilot CLI を使って、初めての変更を公開できました。具体的には、次のことを行いました。

- 機能ブランチで Copilot の会話を開始しました。
- ゲームカードに小さな変更を加えるよう agent に依頼しました。
- `/diff` で変更をレビューしました。
- 転送されたブラウザーでアプリを実行し、星評価を確認しました。
- 最初のプルリクエストを作成してマージしました。

次は、[フィルタリング Issue から開始し、Plan mode と Autopilot mode を使って][next-lesson]、より大きな機能を構築します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
