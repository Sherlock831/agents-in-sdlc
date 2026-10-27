---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習では、Copilot cloud agent にユーザー体験の改善や機能追加に関する 3 つのタスクを依頼しました。Cloud agent は非同期かつ自律的に動作しますが、タスクの進行状況を監視することも重要です。

Cloud agent に割り当てたタスクを管理する方法はいくつかあります。[GitHub.com の agents ページ][agents-page]もその 1 つです。この mission control では、オープンなプルリクエスト（PR）がある agent のタスクをすべて確認できます。実行内容を調べたり、進行中のセッションを誘導したりできます。

このレッスンでは、次のことを行います。

- agents ページで cloud agent のタスクを監視します。
- 進行中のセッションを誘導して、機能の追加を依頼します。

## シナリオ

ハイコントラストモードの作成を agent に依頼した後、チームはこの機会にライトモードも追加することにしました。サイトのスタイルと切り替え機能を更新しているため、あわせて実装するのが合理的です。ハイコントラストに加えてライトモードも追加するよう、agent の作業を誘導します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左ペインの両方にタスクの一覧が表示されます。Copilot に割り当てたタスクとして、次の内容を確認してください。
   - コードのドキュメントの更新
   - ゲームを変更する API の生成
   - Web サイトへのハイコントラストモードの追加
3. 実行中のタスクを選択し、Copilot が実行した作業を確認します。次のような手順が表示される場合があります。
   - リポジトリのチェックアウト
   - Copilot の作業環境の作成
   - MCP サーバーの設定
   - タスクを完了するための各種手順

> [!NOTE]
> 表示される手順は、Copilot の作業状態や選択したアプローチによって異なります。

4. 右側に表示されるプルリクエスト（PR）ペインも確認してください。PR と変更されたファイルを確認し、作業をさらに監視できます。

## Cloud agent を誘導する

実行中のタスクを確認できたので、ハイコントラストモードの作業中にライトモードの切り替え機能も追加するよう Copilot に依頼します。

1. ハイコントラストモードの追加に関するセッションを選択します。正確なタイトルは Copilot が選んだ名前や作業状態によって異なります。

    ![Mission control に表示されたアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業を開始したことが表示されるまで数分待ちます。次のようなメッセージが表示されたら作業開始の目安です。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot が作業中の agent ページで、セッション誘導ダイアログを強調表示した画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に取り込むことを確認します。

## Copilot の作業を待つ

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行していた手順が終わると、新しい依頼を作業に取り込みます。

前回と同じように、完了まで数分かかります。その間に、ここまでに学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent のタスクを監視して誘導する中心的な場所である Copilot agents ページを確認しました。Mission control では、進行中と完了済みのタスクを追跡し、実行中の作業をレビューし、進行中のタスクに追加の指示を出せます。

次のことを確認しました。

- Mission control と agents ページで cloud agent のタスクを監視する
- 進行中のセッションに機能追加を依頼する

Copilot がアクセシビリティ機能の作業を続けている間に、次は [Copilot が作成したプルリクエストを改善][next-lesson]しましょう。

## リソース

- [Copilot agents ページ][agents-page]
- [Custom agent について][custom-agents]

---

| [← 前のレッスン: Custom agent][previous-lesson] | [次のレッスン: Copilot の作業を改善 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
