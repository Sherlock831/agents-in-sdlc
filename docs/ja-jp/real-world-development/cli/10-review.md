---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連のワークフローで GitHub Copilot CLI を使いました。具体的には、次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、フィルタリング Issue を見つけました。
- 星評価を追加し、port 転送したブラウザーで結果を確認して、最初のプルリクエスト（PR）を手動でマージしました。
- フィルタリング Issue から開始し、Plan mode で方法を決め、Autopilot mode で実装し、Interactive mode でレビューしました。
- Custom instruction で agent を導き、既存の `quality-checks` skill をカスタマイズして、unit test、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを確認しました。
- QA custom agent を作成して選択し、要件、カバレッジ、skill の結果、ブラウザーでの根拠を確認しました。
- フィルタリング全体の変更をレビューし、フィルタリング PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意の cloud agent への委任に使う slash command を確認しました。

## 作成したもの

ワークショップには、更新済みの `main` からそれぞれ個別のブランチで作成する、2 つの PR の節目があります。

1. **星評価:** ゲームカードに、既存の `starRating` と未評価状態を明示して表示します。
2. **フィルタリングと品質ワークフロー:** フィルタリングを実装し、instruction を更新して機能に適用し、`quality-checks` レポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

フィルタリングを計画して PR を作成するまで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、関連作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP 経由での Copilot によるブラウザー確認など、複数の方法でコードを確認しました。`quality-checks` skill は unit test、lint、型チェックを実行し、新しい形式でレポートしました。QA はこれらの結果と要件、テストカバレッジのレビューをまとめてから PR に進みました。

追加するテストは、実際に不足しているカバレッジを補うものである必要があります。QA の実行で新しいテストが不要と判断される場合もあります。マージを許可する前に、コードと根拠をレビューしてください。変更があった場合は、影響を受ける根拠を更新します。

## ベストプラクティス

Copilot に与えるコンテキストと tool が作業に影響します。このワークショップでは、instruction の更新、skill のカスタマイズ、QA プロファイルの作成、MCP サーバーの設定を行いました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。Instruction は標準を設定し、skill は繰り返し実行できる作業をまとめ、custom agent は専門家の役割を定義し、MCP サーバーは外部 tool に接続します。agent の要約だけでなく、実際の変更と tool の結果をレビューしてください。

タスクに合わせて **mode と model** を選択します。実装前に方法を検討するには **Plan**、範囲を絞った変更を自分で確認しながら進めるには **Interactive**、範囲の明確なタスクを任せるには **Autopilot** を使います。定型的な編集には高速な model、複雑な作業にはより高性能な model を選びます。

インフラストラクチャと同じくらい、コンテキストも重要です。何を作りたいか、その理由、実現方法を明確に説明すると、出力が大きく変わります。

## さらに試す

主要なワークフローは完了しました。Copilot CLI には、次のような機能もあります。

- `/review` で、コードレビュー agent に変更を分析してもらいます。
- `/rubber-duck` で問題について話し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並列に実行します。
- `/worktree` で別のタスクを独立させます。
- `/delegate` で Copilot cloud agent にタスクを送ります。

## 次のステップ

ツールの使い方を上達させるには、使い続けるのが一番です。製品コード、趣味のコード、何年も作りたいと思いながら手をつけていなかった小さなアプリなどで活用してください。チームで学んだことを共有し、互いの経験から学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張したい場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境も比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
