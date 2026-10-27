---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI の mode、カスタマイズ、MCP tool、プルリクエスト自動化を学びながら、Tailspin Toys の変更を 2 つ実装、検証、リリースします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** は、ターミナルで GitHub Copilot を agent 型コーディングアシスタントとして使えるようにします。コードベースを調べ、コードを生成し、コマンドを実行し、外部 tool に接続できます。すべてコマンドラインから行えるため、グラフィカルエディターに切り替えずに作業を続けられます。

ワークショップでは、Tailspin Toys を使った連続したワークフローを進めます。

1. GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして、プロジェクトを確認します。
2. 星評価を小さく変更し、ブラウザーで結果を確認して、最初のプルリクエスト（PR）を手動でマージします。
3. フィルタリング Issue から開始し、Plan mode で方法を決め、Autopilot mode で構築してから、Interactive mode でレビューします。
4. リポジトリの instruction を更新し、フィルタリング作業に適用します。
5. 既存の `quality-checks` skill をカスタマイズし、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーでフィルタリングを確認します。
7. 品質保証 (QA) custom agent を作成して使い、要件、カバレッジ、検証の根拠をレビューします。
8. フィルタリング全体の変更をレビューし、フィルタリング PR に Agent Merge を使います。
9. コンテキスト、モデル、共有、cloud agent への任意の委任に役立つ slash command を確認します。

このワークショップでは、星評価、フィルタリングと instruction の更新、skill の更新、QA プロファイル、テストという 2 つの PR を作成します。各 tool を試しながら作業を積み重ねられるよう、フィルタリングと品質ワークフローは 1 つの会話とブランチで進めます。

## レッスン

| レッスン | トピック | 説明 |
| ------ | ----- | ----------- |
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証し、使い方を確認する |
| [2. 星評価の追加: すばやい改善][ex2] | 最初の変更 | 既存の評価と null の場合の表示を追加し、最初の PR をマージする |
| [3. Agent mode: Plan と Autopilot][ex3] | Agent mode | Issue を基に機能を計画し、Autopilot で構築して Interactive mode でレビューする |
| [4. Custom instruction で Copilot を導く][ex4] | コンテキスト | instruction を確認して更新し、フィルタリングに適用する |
| [5. quality-checks skill をカスタマイズして使う][ex5] | 繰り返し可能なチェック | 既存の skill を確認してレポート形式を変更し、実行する |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの確認 | CLI で MCP を設定し、フィルタリングの動作を確認する |
| [7. QA agent を作成して使う][ex7] | 要件とカバレッジ | 専門プロファイルを作成して選択し、最終的な検証の根拠を集める |
| [8. 機能 PR を作成してマージする][ex8] | レビューとマージ | 変更全体をレビューし、PR を作成して Agent Merge を使う |
| [9. GitHub Copilot CLI の slash command][ex9] | CLI の機能 | コンテキスト、モデル、共有、cloud agent への任意の委任を確認する |
| [10. 振り返りと次のステップ][ex10] | まとめ | ワークフロー、再利用可能なカスタマイズ、追加リソースを振り返る |
| [任意: Foundry の導入][foundry] | ホスト型 agent | モデルを準備し、カタログに基づく agent をデプロイして Web サイトに接続する |

## 前提条件

このワークショップに参加する前に、次のものを用意してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかの有効なプランがある GitHub アカウント
- [ ] リポジトリと Codespace を作成する権限
- [ ] ターミナル / コマンド ライン操作の基本的な知識

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使う agent、MCP、コードレビュー、Copilot CLI の機能が含まれています。

> [!NOTE]
> Copilot Business または Copilot Enterprise を利用している場合は、管理者が Copilot CLI を有効にしていることを確認してください。

## はじめに

**[前提条件から始める →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
