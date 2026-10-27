---
title: "演習 3 - Custom agent"
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

このレッスンでは、custom agent を使って次のことを確認します。

- Custom agent の定義方法
- Custom agent へのタスクの割り当て方

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを使えるようにすることに取り組んでいます。最近のユーザーフィードバックでは、テキストと背景色のコントラストが不十分なため、ダークテーマが読みにくいという意見がありました。このアクセシビリティの問題に対応するため、デザインチームはユーザーが切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なため、できるだけ早く実装する必要があります。custom agent を使って機能を生成しましょう。

## アクセシビリティ custom agent を確認する

アクセシビリティ用の custom agent はすでに用意されています。内容を確認し、Copilot の作業をどのように導くかを理解しましょう。

Codespace に戻り、次の手順でアクセシビリティ custom agent のファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML frontmatter を確認します。

> [!CAUTION]
> `name` と `description` を含む frontmatter は、custom agent に必須です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコードを生成する際の主な役割
   - アクセシビリティのベストプラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題や誤り

## Issue を作成して割り当てる

Mission control は、環境内のすべての agent を操作する中心的な場所です。Copilot cloud agent にタスクを割り当て、進行状況を監視し、追加の指示を出せます。まず、ハイコントラストモードを作成するタスクを Copilot に割り当てましょう。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択し、新しい Issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次のテキストを入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

    ![Custom agent と Accessibility が強調表示された cloud agent の割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に特化した AI アシスタントである GitHub Copilot の [custom agent][custom-agents]を確認しました。Custom agent を使うと、チームの専門知識や標準を再利用可能な agent にまとめ、特定の種類の作業を効果的に実行するよう Copilot を導けます。

次のことを確認しました。

- Custom agent の定義方法
- Custom agent へのタスクの割り当て方

Copilot がハイコントラストモードを実装している間に、[mission control から agent セッションを監視して誘導][next-lesson]しましょう。

## リソース

- [Custom agent について][custom-agents]
- [組織で custom agent を使うための準備][org-custom-agents]
- [Enterprise で custom agent を使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
