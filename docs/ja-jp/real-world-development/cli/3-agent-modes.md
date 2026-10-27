---
title: "レッスン 3 - Agent mode: Plan と Autopilot"
description: "Plan でアプローチを決め、Autopilot で Issue に基づくフィルタリング機能を構築し、Interactive mode で結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

まず小さな機能をプロジェクトに追加しました。より大きな変更には、さらにしっかりしたプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに沿って作業できるため、適切な方法で適切なものを構築できます。ここから数回のレッスンでは、Issue を起点に機能を開発し、コードの妥当性と機能の動作を確認し、最終的にプロジェクトにマージするという、一般的な agent 主導の開発プロセスを進めます。

> [!NOTE]
> この機能のワークフローでは、同じ会話とブランチを続けて使います。通常、ファイルの種類ごとに異なるブランチや PR を使いますが、ここでは主要な概念に集中するため簡略化しています。

このレッスンでは、次のことを行います。

- GitHub Issue から新しい Copilot の会話を開始します。
- Plan mode で要件を決めます。
- Autopilot mode を使って新しい機能を実装します。
- コードをレビューします。
- port 転送したブラウザーで機能を手動で検証します。

この機能の作業を続けながら、リポジトリの instruction を更新し、既存の `quality-checks` skill をカスタマイズし、MCP を使った検証を追加し、QA agent を作成して、機能の PR を作成します。

## シナリオ

Tailspin Toys のカタログが増えており、訪問者がカテゴリと publisher でゲームを絞り込める必要があります。バックログの Issue には機能の概要が記載されていますが、カテゴリを組み合わせる方法など、コーディング前に合意しておくべき詳細があります。Plan mode で決定し、その後、範囲を明確に定めて Autopilot に実装を許可します。

## 背景

AI コーディング agent を開発フローに導入しても、基本は変わりません。むしろ、さらに重要になります。多くの開発者は、次のような流れで作業します。

1. 作業内容を説明する Issue を作成します。
2. 構築するものの計画を立てます。
3. コードを構築してレビューします。
4. テストを実行してコードを検証します。
5. 新しい機能を手動で検証します。
6. プルリクエスト（PR）を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 詳細はチームや組織によって異なりますが、ほとんどの場合は上記の流れに沿っています。

標準的なアプローチに従うことで、AI が生成したコードが要件を満たし、手作業で書いたコードと同じレビューのプロセスを通るようにできます。

## 会話モード

**会話モード**は、agent にどの程度の自律性を与えるかを決めます。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すと、モードを切り替えられます。

- **Interactive**: ユーザーと agent が協力して進めます。agent は変更を提案し、次に進む前にユーザーの入力を待ちます。
- **Plan**: agent が最初に計画を作成します。プロジェクトファイルの編集はできません。
- **Autopilot**: agent が自律的に作業し、コードを書いてテストを実行し、タスクが完了するまで反復します。

最初に Plan mode で計画を確認し、その後 Autopilot で実装します。

## Issue から開始する

フィルタリングの作業を始める前に、Codespace に戻り、リポジトリとターミナルを準備します。

1. Codespace に戻ります。停止している場合は、再起動してから続けます。
2. 星評価の PR がマージされていることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルタリング作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーを使って **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7`（または同様の番号）が表示されます。`#` を使うと、GitHub 上の Issue やプルリクエスト（PR）をコンテキストに追加できます。

## フィルタリング機能を計画する

計画を立てると、Copilot に実装を任せる前に、機能やタスクのアプローチを決められます。複雑な作業では、時間をかけて計画することをお勧めします。Plan mode に切り替えて、Copilot に計画を作成してもらいましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan mode に切り替えます。プロンプトの下に **Plan** が表示されることを確認します。
2. 前の手順で追加した Issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、最適な方法を決めて計画を作成します。

3. フィルタリングの動作について Copilot から質問されたら、希望する動作を回答します。正解は 1 つではありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して、計画を展開します。
5. 上下にスクロールして計画をレビューします。
6. 決定と合わない箇所があれば、変更を依頼します。

## Autopilot を承認する

計画を作成してレビューできました。次は実装です。Copilot に Autopilot mode で作業してもらいましょう。

Autopilot は、タスクが完了したと agent が判断するまで、繰り返し作業します。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みのバージョンに表示される同様の項目を選択します。
2. プロンプトの下に **Autopilot** が表示されることを確認します。
3. Copilot が合意した計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まる場合があります。先に計画をレビューしてください。依存関係が不足している、または port が競合していると Copilot が報告した場合は、チェック完了と判断する前に環境の問題を解決します。

## 実装をレビューして検証する

コードが生成されたら、他のコードと同じように、マージする前にレビューする必要があります。コードをレビューし、サイトを起動して動作を確認します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive mode に切り替えます。モード表示に **Plan** または **Autopilot** がないことを確認します。
2. `/diff` を入力し、フィルタリングの実装とテストを確認します。
3. Issue と、計画中に決めた内容を照らし合わせます。
4. コードをレビューしたら <kbd>Esc</kbd> を押して差分表示を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新しい機能を試す

コードに問題がないように見えても、実行できるとは限りません。前と同じようにアプリを起動し、Codespaces で port を転送してサイトを開きましょう。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces で port `4321` が利用可能になったら **Open in Browser** を選択します。
3. 計画で決めたカテゴリと publisher のフィルタリング、その組み合わせを試します。
4. リセットと一致する結果がない場合の動作が、Issue と決定した内容に合っていることを確認します。
5. Codespace に戻り、Copilot に起動した dev server の停止を依頼します。

## まとめと次のステップ

異なる会話モードを使って、機能を構築しレビューしました。このレッスンでは、次のことを行いました。

- GitHub Issue から新しい Copilot の会話を開始しました。
- Plan mode で要件を決めました。
- Autopilot mode を使って機能を実装しました。
- コードをレビューしました。
- port 転送したブラウザーで機能を手動で検証しました。

次は、[custom instruction][next-lesson]を使って、コード生成時に文書化されたプラクティスに従うよう Copilot を導きます。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンドリファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
