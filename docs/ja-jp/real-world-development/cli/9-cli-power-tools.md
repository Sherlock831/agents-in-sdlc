---
title: "レッスン 9 - GitHub Copilot CLI の slash command"
description: "コンテキスト管理、モデルの選択、セッションの共有、任意のクラウドへの委任に使う slash command を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

一般的な CLI ツールと同様に、GitHub Copilot CLI には対話に使う slash command が多数用意されています。高度な機能、内部情報、追加の設定項目を利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、ほかの便利な command をいくつか確認しましょう。

このレッスンでは、次のことを行います。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` を使い、利用可能なモデルを確認します。
- `/share` でセッションを export または共有する方法を学びます。
- 並列作業、worktree、cloud agent への委任に使う任意の command を確認します。

## シナリオ

CLI の主なワークフローを完了しました。次は、コンテキスト管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent]への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクに取り組むと、モデルの最大コンテキストウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、slash command でコンテキストを確認したり、自分で圧縮したりできます。

1. Codespace に戻ります。Copilot CLI が起動していない場合は、リポジトリルートから起動します。
2. 次の command を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在の token 使用量、システム instruction、tool、メッセージ、空き領域にコンテキストがどのように分けられているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話がすでに短い場合は、大きな違いがないこともあります。

> [!NOTE]
> Copilot CLI は、コンテキストウィンドウが埋まると自動的にコンテキストを圧縮します。自分でタイミングを選ぶ場合は `/compact` を使います。関係のない別のタスクに切り替えるために新しい会話を始める場合は、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルによって得意なことが異なり、開発者ごとに好みも違います。Copilot CLI では、利用できるモデルを一覧表示し、使うモデルを選択できます。

1. 次の command を入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況に関する情報を確認します。
3. 現在のモデルを使うか、別のモデルを選択します。または <kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力し、学んだことを共有すると、AI ツールの活用方法を互いに改善できます。`/share` command を使うと、セッションを Markdown または HTML ファイルに export したり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みのバージョンにある `/share` の選択肢を確認します。
2. セッションを共有する場合は、必要に応じて `/share file` などを選択して、ローカルの Markdown ファイルに export します。
3. export した内容を誰かに送信したり公開したりする前に、内容を確認します。セッションの export には、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容は公開しないでください。

## 任意: 作業の規模を広げる、または委任する

ワークショップの主要な部分は完了しました。Copilot CLI には、大きなタスク向けの command も用意されています。

- `/fleet` は独立したサブタスクを subagent に割り当て、並列に実行できます。
- `/worktree` は別のタスク用に独立した Git worktree を作成できます。
- `/delegate` は Copilot cloud agent にタスクを送り、非同期に作業させたり、プルリクエストを作成させたりできます。

追加の worktree やリモート作業が発生するため、これらの command は任意です。試すときは範囲の明確な新しいタスクから始め、通常のワークフローに沿って結果をレビューしてください。非同期 agent 作業について詳しく知りたい場合は、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI の slash command を使うと、設定を変更したり、セッションを共有したり、内部での動作を確認したりできます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する方法を確認しました。
- `/model` を使い、利用可能なモデルを確認しました。
- `/share` でセッションを export または共有する方法を学びました。
- 並列作業、worktree、cloud agent への委任に使う任意の command を確認しました。

Copilot CLI にはほかにも多くの slash command があります。次は、ここまでに学んだことを[振り返り][next-lesson]、さらに学ぶための手順を確認しましょう。

## リソース

- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
