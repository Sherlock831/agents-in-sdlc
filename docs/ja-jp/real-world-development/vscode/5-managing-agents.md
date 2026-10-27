---
title: "演習 5 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: Custom agent][previous-lesson] |
|:--|

GitHub Copilot を agent mode で使うと、コードベースの調査、変更の提案、ファイルの編集、コマンドの実行を自律的に行います。自分のマシンでリアルタイムに実行されるため、VS Code の Copilot Chat には各 tool の呼び出しやファイルの編集がその都度表示されます。差分を確認し、変更ごとに採用または取り消し、エディターを離れずに追加のプロンプトを送信して作業の内容を調整できます。

この演習では、次のことを行います。

- アクセシビリティ agent の作業をリアルタイムで監視します。
- 提案されたファイル編集を確認して採用します。
- セッションの途中で agent を誘導し、ハイコントラストモードに加えてライトモードの切り替えを追加します。

## シナリオ

[演習 4][previous-lesson]で使ったアクセシビリティ custom agent が、ハイコントラストモードを実装しています。作業中に、デザインチームから追加の依頼がありました。既定のテーマが暗すぎると感じるユーザー向けに、ライトモードの切り替えも追加してほしいとのことです。最初から新しい会話を始める代わりに、進行中のセッションを誘導して機能を拡張します。

## Copilot の進行状況を監視する

Copilot Chat ビューには、agent が実行する各手順が表示されます。確認するファイル、実行するコマンド、提案する編集がわかります。ファイルの編集は差分と **Keep** / **Undo** の操作とともにチャット内に表示されます。

1. 演習 4 で使った Copilot Chat ビューに戻ります。
2. agent の作業中、チャットをスクロールして各 tool の呼び出しを確認します。ファイルの読み取りと編集、実行を求められたターミナルコマンドが表示されます。
3. agent がファイル編集を提案すると、差分がチャット内に表示されます。
   - **Keep** を選択すると変更を採用します。
   - **Undo** を選択すると変更を取り消し、agent に再度作業させます。
4. チャットの上部にある **Files changed** を開くと、このセッション中に agent が変更したすべてのファイルを確認できます。

> [!NOTE]
> ターミナルコマンドの実行などで tool の権限が必要になると、agent は一時停止して **Allow** を表示します。内容を確認し、許可してもよいコマンドのみ承認してください。

5. agent がハイコントラストモードの実装を完了するまで待ちます。最後に、変更内容をまとめたメッセージが表示されます。

## ライトモードを追加するよう agent を誘導する

ハイコントラストモードの準備ができたので、同じ会話でライトモードの切り替えも追加します。同じチャットセッションを続けるため、agent は前の作業から得たコンテキスト（編集した component、採用したスタイル、設定した local storage のパターンなど）を保持しています。

1. Copilot Chat の入力欄に、次の追加プロンプトを送信します。

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. agent はすでに加えた変更を確認し、ライトモードの追加を提案します。
3. ハイコントラストモードと同じように、新しい編集を確認し、差分ごとに **Keep** または **Undo** を選択します。
4. agent が意図しない方向に進んだ場合（たとえば、切り替え UI を望まない方法でリファクタリングする場合）は、チャット入力欄の隣にある **Stop** ボタンで停止し、明確な追加プロンプトを送信します。

> [!TIP]
> 進行中のセッションを誘導すると、最初からやり直すよりもすばやく作業できます。追加プロンプトを使って、調整（「CSS を簡潔にし、selector を重複させず CSS variable を使ってください」）、方向修正（「3 つ目の toggle は追加せず、ライトモードを既存の設定パネルに組み込んでください」）、範囲の拡張（「両方の toggle にキーボードショートカットも追加してください」）を依頼できます。

## 変更されたファイルをレビューする

作業を commit する前に、agent が変更したファイルを確認します。

1. VS Code で **Source Control** ビューを開きます。
2. 変更されたファイルの一覧を確認します。Astro component、スタイル、関連テストなどが更新されているはずです。
3. 変更されたファイルをいくつか開き、差分を確認します。custom agent のアクセシビリティパターン（ARIA 属性、キーボード操作、セマンティック HTML、local storage による設定の保持）が反映されていることを確認します。
4. 問題がなければ、Source Control パネルで変更を stage して commit します。次のレッスンで公開します。

## まとめと次のステップ

Copilot Chat agent mode を使い、進行中のセッションを監視して誘導しました。変更されるたびにファイル編集をレビューし、変更を採用または取り消し、コンテキストを保ったまま追加プロンプトを送信して作業を拡張しました。

このレッスンでは、次のことを確認しました。

- agent の作業をリアルタイムで監視する
- ファイル編集を個別に採用または取り消す
- 作業中のセッションに追加プロンプトを送り、誘導する

[次のレッスン][next-lesson]では、アクセシビリティ作業をプルリクエストとして公開し、このラボでローカル custom agent が作成した変更をレビューします。

## リソース

- [VS Code の Copilot Chat][copilot-chat-vscode]
- [Agent mode の使用][agent-mode]

---

| [← 前のレッスン: Custom agent][previous-lesson] | [次のレッスン: Copilot の作業を改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
