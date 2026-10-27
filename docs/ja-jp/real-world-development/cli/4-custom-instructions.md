---
title: "レッスン 4 - Custom instruction で Copilot を導く"
description: "リポジトリの instruction を確認し、ドキュメント標準を追加して、フィルタリングコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行する必要がある場合は、そのガイダンスを Copilot が利用できるようにします。[Instruction file][instruction-files]を使うと、必要なコードだけでなく、その構成方法も Copilot に伝えられます。フィルタリング機能を実装できたので、Copilot が使った instruction を確認し、ドキュメント標準を追加して、コードに適用しましょう。

このレッスンでは、次のことを行います。

- リポジトリの instruction とパス単位で適用される instruction file が agent に伝わる仕組みを確認します。
- コーディング標準に従うよう instruction file を更新します。
- instruction file がコードに与える影響を確認します。

## シナリオ

優れた開発チームと同じように、Tailspin Toys にも開発プラクティスに関するガイドラインと要件があります。たとえば、次のような内容です。

- コメントにはコードの内容を言い換えるのではなく、意図とわかりにくい設計上の判断を記述します。
- `db/` と `src/lib/` にある export された関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を追加します。`db` 引数を注入する場合は、そのことも記載します。
- 再利用可能な Astro component は `Props` の契約を説明し、関連コードが変更されたときにコメントも最新の状態に保ちます。
- 既存のフォーマットと lint のガイダンスを維持します。

instruction file を使うことで、ここで取り上げたプラクティスに沿ってタスクを実行できるよう、Copilot に適切な情報を提供します。

## Instruction file

Custom instruction を使うと、コンテキストや設定を Copilot に渡し、コーディングスタイルや要件を理解させることができます。これは、より適切な提案やコードスニペットを得るのに役立ちます。好みのコーディング規約やライブラリ、コードに含めたいコメントの種類を指定できます。リポジトリ全体向けにも、特定のファイル種別向けにも作成できます。

instruction file には次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の instruction file です。ほとんどのリクエストに関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスクに関するガイドラインを提供します。

> [!NOTE]
> ほかの instruction 形式とサポート状況は、環境によって異なります。特定の形式に依存する前に、[custom instruction のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトの custom instruction file を確認する

スターター用のプロジェクトには、すぐに始められるよう instruction file がすでに用意されています。変更を加える前に、どのようなファイルがあるかを確認しましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡潔な説明とコーディングガイダンスを確認します。この instruction は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどの instruction があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、instruction を適用するファイルを指定する glob です。
6. `.github/instructions/drizzle.instructions.md` を開き、他の instruction file やプロジェクト内の既存ファイルへのリンクを確認します。大きな instruction セットを小さく再利用しやすいファイルに分け、Copilot に参考となる例を示せます。

## チームのガイダンスに合わせて instruction file を更新する

すでに用意されているファイルはよい出発点ですが、まだ不足があります。中心となる `copilot-instructions.md` を更新し、新しく生成される TypeScript に TSDoc comment が追加されるようにします。

1. `.github/copilot-instructions.md` で **Code formatting guidance** という見出しのセクションを探します。35 行目付近にあります。
2. 最後の箇条書きとして、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの instruction を読み込みます。更新した instruction を読み込ませつつ、機能のコンテキストを保つために、フィルタリングの会話を終了して再開します。

1. フィルタリングの会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートで `game-filters-cli` ブランチをチェックアウトした状態から、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示された場合は、フィルタリングの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc comment が追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が instruction file からコンテキストを取得する仕組みを確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリの instruction とパス単位で適用される instruction file が agent に伝わる仕組みを確認しました。
- コーディング標準に従うよう instruction file を更新しました。
- instruction file がコードに与える影響を確認しました。

次は、再利用可能な `quality-checks` skill を[カスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI 用の custom instruction を追加する][instruction-files]
- [Custom instruction のサポート][custom-instructions-support]
- [Custom instruction を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
