---
title: "演習 4 - Custom agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## Custom agent とは

GitHub Copilot の [custom agent][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成し、重点を絞った指示、ベストプラクティス、コーディングパターン、分野固有の知識を定義すると、Copilot がその作業を効果的に実行できるようになります。チームの専門知識を再利用可能な agent にまとめることもできます。たとえば、[WCAG][wcag]への準拠を確認するアクセシビリティ agent、安全なコーディングプラクティスに従うセキュリティ agent、一貫したテストパターンを維持するテスト agent などです。

Custom agent は、プロジェクト内の `.github/agents` フォルダーにある Markdown ファイル、またはグローバルな `~/.copilot/agents` に定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、その agent の動作、専門知識、指示を記載する Markdown プロンプトが必要です。

### Custom agent と agent skill の比較

Custom agent と [agent skill][agent-skills-concept]には共通点があります。どちらも主に Markdown ファイルで定義され、AI に作業の方法を伝えます。わかりやすく区別するなら、**custom agent は作業者、skill はツール**です。

Custom agent は独自のコンテキストウィンドウを持ち、作業を進めるために skill（さらには別の agent）を統括します。このラボのアクセシビリティ custom agent は、アクセシビリティガイドラインに沿ってサイトをレビューし、更新します。その作業の一環として、プルリクエストワークフロー用やテストの実行・管理用の skill を呼び出すこともできます。

> [!NOTE]
> Custom agent の作成に唯一の正解はありません。AI を使うほかの場面と同様に、環境やシナリオに合わせてテストと調整を行ってください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを使えるようにすることに取り組んでいます。最近のユーザーフィードバックでは、テキストと背景色のコントラストが不十分なため、ダークテーマが読みにくいという意見がありました。このアクセシビリティの問題に対応するため、デザインチームはユーザーが切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なため、できるだけ早く実装する必要があります。custom agent を使って機能を生成しましょう。

この演習では、次のことを行います。

- 既存のアクセシビリティ custom agent を確認します。
- Copilot Chat でアクセシビリティ agent を使い、ハイコントラストモードを実装します。

## アクセシビリティ custom agent を確認する

アクセシビリティ用の custom agent はすでに用意されています。内容を確認し、Copilot の作業をどのように導くかを理解しましょう。

Codespace に戻り、ターミナルを開いて `main` からアクセシビリティ作業用の新しいブランチを作成します。演習 3 のフィルタリング PR とは分けて作業します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML frontmatter を確認します。

> [!CAUTION]
> `name` と `description` を含む frontmatter は、custom agent に必須です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコードを生成する際の主な役割
   - アクセシビリティのベストプラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題や誤り

## Copilot Chat で custom agent を使う

VS Code では、`.github/agents` に定義されている各 custom agent が Copilot Chat ビュー下部の agents dropdown に表示されます。custom agent を選ぶと、その agent の instruction と tool を使うチャットセッションになります。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は手順で説明します。

1. すでに選択されていない場合は、Chat ビューの agent dropdown で **Agent** を選択します。

   ![Chat ビューの agent picker を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. Chat ビューの下部にある agents dropdown を選択します。現在の agent 名が表示されます。既定値は **default** です。
3. 利用可能な agent の一覧から **Accessibility agent** を選択します。
4. アクセシビリティ agent に次のプロンプトを送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調べ、変更を提案してプロジェクトファイルに適用します。変更はファイルパスと差分とともにチャット内に表示されます。
6. agent が変更したファイルの一覧は **Files changed** の表示で確認できます。

> [!NOTE]
> 作業には数分かかることがあります。Copilot はリポジトリに実際の変更を加えます。Astro component、CSS、関連するテストなどの既存ファイルを編集する場合があります。

次の演習では、進行中の作業をレビューし、Copilot を誘導します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に特化した AI アシスタントである GitHub Copilot の [custom agent][custom-agents]を確認しました。Custom agent を使うと、チームの専門知識や標準を再利用可能な agent にまとめ、特定の種類の作業を効果的に実行するよう Copilot を導けます。

次のことを確認しました。

- Custom agent の定義方法
- Copilot Chat の agent mode で custom agent を使う方法

次は [agent の作業を監視して誘導][next-lesson]します。変更を確認し、同じセッションにライトモードの切り替え機能を追加します。

## リソース

- [Custom agent について][custom-agents]
- [IDE で custom agent を作成する][creating-custom-agents-ide]
- [VS Code の custom agent][custom-agents-vscode]
- [Custom agent の設定][custom-agents-config]
- [Awesome Copilot の custom agent][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
