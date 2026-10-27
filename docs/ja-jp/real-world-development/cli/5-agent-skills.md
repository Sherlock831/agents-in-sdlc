---
title: "レッスン 5 - quality-checks skill をカスタマイズして使う"
description: "既存の quality-checks skill を確認してレポート形式をカスタマイズし、フィルタリング機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、開発にはさまざまな作業があります。これまで、コードを手動で検証し、instruction file を使って標準に従うよう Copilot を導いてきました。テスト、lint、継続的インテグレーション（CI）などはどうでしょうか。

このような作業には **agent skill** が適しています。Skill を使うと、こうした操作を正しく実行する方法を Copilot に伝えられます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` skill を確認します。
- 結果のレポート形式をカスタマイズします。
- skill を再読み込みして実行します。

## シナリオ

Tailspin Toys では、unit test、lint、型チェックに `quality-checks` skill を使っています。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## Instruction、script、resource

Agent skill は、再利用可能なタスクの instruction、実行可能な script、補助 resource をまとめたもので、agent が必要に応じて読み込みます。基本的には、skill 名を持つフォルダーと、その中の `SKILL.md` という Markdown ファイルで構成されます。Markdown には、skill の名前と説明を定義する frontmatter、その役割の概要、呼び出すべき状況についてのガイダンスを記載します。skill のフォルダーに、実行時に使う script やその他の resource を含むサブフォルダーを追加することもできます。

> [!NOTE]
> Skill に追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` skill は、プロジェクトにある既存の command を使うため、`SKILL.md` のみで構成されています。

Skill は、チームで共有して再利用できるリポジトリ内の `.github/skills` フォルダーや、ユーザーの skill 用フォルダー `~/.copilot/skills` に配置できます。

## Skill を確認する

Tailspin Toys チームが unit test、lint、型チェックの実行用に作成した `quality-checks` skill を確認しましょう。

1. Codespace に戻ります。Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭にある `name` と `description` を読みます。Description は、Copilot が skill を呼び出す状況を判断するのに役立ちます。
3. 指示を読み、テストと lint の手順をどのように案内しているかを確認します。
4. **Results output formatting** セクションがまだないことを確認します。

## 変更を加える前に skill を実行する

Skill は Copilot CLI から直接呼び出すことも、自然言語を使って呼び出すこともできます。3 種類のチェックを実行するよう Copilot に依頼しましょう。

1. Interactive mode のフィルタリング会話に戻ります。
2. 次のプロンプトを送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に出力されるレポートを確認します。

## レポートをカスタマイズする

何を実行したか、成功したか、tool が実際に報告した内容を確認できるレポートに改善しましょう。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. 次のセクションをファイルの末尾に追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新した skill を実行する

変更を加えたので、動作を確認しましょう。Copilot CLI では、会話を再起動せずに編集した skill を再読み込みできます。

1. 次の command を入力します。

   ```plaintext
   /skills reload
   ```

2. 前回とまったく同じプロンプトを送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に出力されるレポートを確認し、最初の結果と比較します。

## まとめと次のステップ

既存の agent skill をカスタマイズして使用しました。このレッスンでは、次のことを行いました。

- unit test、lint、型チェックを実行する `quality-checks` skill を確認しました。
- 結果のレポート形式をカスタマイズしました。
- skill を再読み込みして実行しました。

この変更は、機能 PR のフィルタリング作業とともに含まれます。次は、[Playwright MCP サーバー][next-lesson]を使って Copilot にサイトを直接操作させます。

## さらに見る skill の例

次の community example は参照用であり、追加の作業ではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図と export script: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
