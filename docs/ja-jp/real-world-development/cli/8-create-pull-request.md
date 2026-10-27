---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルタリング、instruction、skill の更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、instruction の更新、skill の更新、品質保証（QA）プロファイル、テストは、すべて 1 つのブランチに保存されています。まとめてレビューし、プルリクエストを作成しましょう。星評価のプルリクエスト（PR）は手動でマージしました。今回は **Agent Merge** にマージ処理を任せます。

> [!NOTE]
> 通常は、機能、instruction の更新、skill の更新、QA agent をそれぞれ別の PR に分けます。ワークショップを簡潔にするため、フィルタリングと品質ワークフローに関する作業を 1 つの会話とブランチにまとめ、この PR に含めました。

このレッスンでは、次のことを行います。

- Agent Merge とは何か、PR のマージ処理を自動化する方法を学びます。
- 機能全体の変更と検証の根拠を確認します。
- フィルタリングの PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルタリングのワークフローを通じて、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys は、マージを許可する権限を開発者の管理下に置きながら、PR の残りの作業を自動化したいと考えています。

## Agent Merge の紹介

**Agent Merge** は、プルリクエストをマージするまでに必要な残りの作業を自動化します。有効にすると、Copilot は CI チェックの失敗やレビューコメントへの対応、必要な場合の rebase など、PR をブロックしている問題に取り組みます。その後、リポジトリで許可されていれば GitHub の auto-merge を有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその役割を任せることもできます。Agent Merge を有効にする前に、作業をレビューして問題がないことを確認してください。

## Agent Merge で PR を管理する

コードの作成が終わったら、全体をレビューし、PR を作成して、残りの作業を Agent Merge に任せましょう。

1. Codespace に戻ります。
2. `/agent` を入力して agent ダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを監視します。すべて成功したら、マージを実行します。
7. `PR #14 was squash-merged successfully.` のような Copilot のメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリの設定、権限不足を回避しません。処理がブロックされた場合は、報告された理由を確認し、リポジトリで許可されているときは、レビュー済みの変更を手動でマージしてください。

## まとめと次のステップ

コードの生成やテストと検証に加えて、プルリクエストのプロセスも自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge とは何か、PR のマージ処理を自動化する方法を学びました。
- 機能全体の変更と検証の根拠を確認しました。
- フィルタリングの PR を作成しました。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、任意のクラウドへの委任などに役立つ [Copilot CLI の slash command][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
