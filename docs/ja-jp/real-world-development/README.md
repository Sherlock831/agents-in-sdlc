---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用しましょう。変更の計画、コードの作成、動作のテスト、プルリクエストのレビュー、agent を使った作業管理を反復して行います。

## シナリオ

あなたは、開発者向けのテーマを使ったボードゲームのクラウドファンディングを提供する架空の会社、Tailspin Toys に入社したばかりの開発者です。チームのバックログは GitHub Issue として登録済みで、作業を選べる状態です。フィルタリングやページネーションなどの機能開発に加えて、アクセシビリティやコーディング標準などの品質改善項目があります。

## 作業環境を選ぶ

GitHub Copilot は、どの環境でも利用できます。希望する開発方法に合わせて環境を選択してください。各ワークショップは個別のセットアップから始まり、共通の Tailspin Toys シナリオを使います。

### [GitHub Copilot CLI][cli]

ターミナルで agent 型アシスタントを使います。Plan と Autopilot mode、instruction、skill、custom agent、Playwright MCP、Agent Merge、実用的な slash command を試します。

### [GitHub Copilot app][app]

デスクトップ app で複数の agent セッションを並行して実行し、セッションモードを切り替え、Canvas で共同作業し、GitHub Issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub Issue を非同期 agent に割り当て、作業を誘導して進行状況を監視し、作成されたプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに、Copilot Chat agent mode、MCP サーバー、custom agent を使って作業します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
