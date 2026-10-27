---
title: "演習 1 - Custom instruction (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにする必要があります。Copilot に特定のコンテキストを渡す方法はいくつかあります。その中心となるのが [instruction file][instruction-files]です。コード生成の指針を Copilot に伝えます。

この演習では、次のことを行います。

- リポジトリの custom instruction とパス単位で適用される instruction file を通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- *現在の* instruction を使い、フィルタリング機能の最初のデータスライス（publishers helper）を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加します。
- 同じプロンプトを再実行し、生成コードが新しい標準を取り入れる様子を確認します。
- instruction の更新とフィルタリング機能の一部を `main` に commit し、次の演習で Copilot が更新されたガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードは、設定した標準から外れる場合があります。Copilot の動作は非決定的です。この演習の目的は、instruction を更新した後の動作の*傾向*を確認することであり、出力を 1 文字ずつ一致させることではありません。

## Instruction file

### シナリオ

優れた開発チームと同じように、Tailspin Toys にも開発プラクティスに関するガイドラインと要件があります。たとえば、次のような内容です。

- データレイヤーには必ず unit test が必要です。
- UI はダークモードで、モダンな印象にする必要があります。
- コードのドキュメントは TSDoc の doc comment として追加する必要があります。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加する必要があります。

instruction file を使うことで、ここで取り上げたプラクティスに沿ってタスクを実行できるよう、Copilot に適切な情報を提供します。

### Custom instruction

Custom instruction を使うと、コンテキストや設定を Copilot に渡し、コーディングスタイルや要件を理解させることができます。これは、より適切な提案やコードスニペットを得るのに役立ちます。好みのコーディング規約やライブラリ、コードに含めたいコメントの種類を指定できます。リポジトリ全体向けにも、特定のファイル種別向けのタスクレベルのコンテキストとしても作成できます。

instruction file には次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の instruction file です。使用する技術スタック、開発中のものの概要、ベストプラクティスなど、ほとんどのチャットや CLI リクエストに関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別向けに作成できます。TypeScript や Astro などの言語、UI component の作成、unit test の追加など、具体的なガイドラインを提供できます。

> [!NOTE]
> IDE で作業する場合、instruction file は Copilot Chat でのコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの instruction file と、`applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は [ほかの標準に基づく instruction file][custom-instructions-support]もサポートしており、AGENTS.md や CLAUDE.md ファイルも利用できます。

### Instruction file を管理するためのベストプラクティス

instruction file の作成に関する詳しい説明はこのワークショップの範囲外です。ただし、サンプルプロジェクトの例は代表的な方法を示しています。概要は次のとおりです。

- `copilot-instructions.md` には、開発中のものの説明、プロジェクト構造、全体に適用するコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` ファイルは、ファイル種別（unit test、Astro component、データレイヤー）や特定のタスク向けの具体的な指示に使います。
- 自然な言葉で明確に記述し、コードの望ましい例と望ましくない例を示します。

instruction file の作成にも AI の使い方にも、唯一の正解はありません。実際に試しながら、プロジェクトに合った方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な instruction file を用意しましょう。このプロジェクトには [UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に instruction file を作成してもらうこともできます。各 surface で方法は異なります（たとえば、VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init` など）。必要な場面では、利用中の surface のレッスンで説明します。
>
> テンプレートや出発点を探していますか。[awesome-copilot][awesome-copilot]には、instruction file、custom agent、その他のリソースがまとめられています。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトの custom instruction file を確認する

このリポジトリに用意されている instruction file を読みます。中心となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡潔な説明に加えて、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の中にある **GitHub Actions Workflows** のガイダンスにも注目してください。これらの内容は Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどの instruction があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、instruction の適用対象ファイルを決める glob（リポジトリルートからの相対パス）です。この例では、`**/*.test.ts` に一致するファイルなど、TypeScript のテストファイルが対象になります。
5. このプロジェクトの unit test 作成に特化した instruction を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの別の instruction file や、プロジェクト内の既存ファイルへのリンクがあります。大きな instruction セットを小さく再利用しやすいファイルに分け、コード生成時に Copilot が参考にできる例を示せます。（ここに記載されたパスは、リポジトリルートではなく instruction file からの相対パスです。）

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメント化はまだ必須ではありません。次の手順で、TSDoc の doc comment とファイルコメントヘッダーのルールを追加します。

## 変更用ブランチを作成する

ここではベストプラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. ブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## Instruction を更新する*前に* Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は手順で説明します。

Custom instruction の効果を確認するため、まず現在の instruction file を使ってプロンプトを送信します。その後、instruction を更新して同じプロンプトを再送信し、違いを確認します。

1. 前の演習で開いたエディターのタブを閉じ、Copilot が必要なコンテキストだけを使うようにします。
2. helper の配置先を示すため、`src/lib/publishers.ts` を開きます。
3. Chat ビューの agent dropdown で **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューの agent picker を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを確認し、コードを更新します。helper とテストなど、複数のファイルが変更されることがあります。
6. 生成された helper が型付き関数として実装され、最初の引数に `db` client を受け取り、型付きの publisher 配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約によるものです。
7. 生成されたコードに TSDoc の doc comment とファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は非決定的に動作するため、指示がなくても doc comment を追加する場合があります。その場合も問題ありません。instruction を更新した後、より一貫して doc comment が追加されることが、この演習の目的です。

## リポジトリ全体に適用する標準を追加する

`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供します。コーディング標準を記載し、コードの提案を改善しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. **Code formatting requirements** セクションを探します。27 行目付近にあります。プロジェクトのコーディング標準は記載されていますが、コード内のドキュメント化に関するルールがないため、生成された helper には doc comment がありませんでした。
3. 既存の標準の直後に次の Markdown 行を追加し、ファイルコメントヘッダーと TSDoc の doc comment を追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、instruction file は全体的なガイダンス向けのリポジトリレベル（`.github/copilot-instructions.md`）にも、特定の言語、ファイル種別、タスク向けの `*.instructions.md` としても作成できます。今回追加した doc comment のルールのように、プロジェクト全体に適用する標準はリポジトリレベルのファイルに記載します。

## プロンプトを再実行して変化を確認する

instruction に doc comment のルールを追加したので、先ほどのプロンプトを Copilot Chat にもう一度送信します。新しい標準によってコードがどのように変わるかを確認しましょう。

1. Copilot Chat に戻り、**New Chat** を選択して会話をクリアします。
2. `src/lib/publishers.ts` を選択し、Copilot が正しいファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc の doc comment が含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

これで新しいプロジェクト標準に従うよう Copilot を誘導し、次の演習で使うコードに適用できました。

## Instruction を commit してブランチを push する

Instruction file もソース管理で管理するファイルです。他のファイルと同じ方法で変更を commit してブランチを push します。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルウィンドウを開きます。
2. ターミナルで変更を確認します。

   ```bash
   git status
   ```

3. instruction の更新と helper の基盤として Copilot が変更したファイルを stage して commit します。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の関連ファイル（テストなど）が表示された場合は、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリに push します。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチを push したので、プルリクエストを作成し、バックログにあるドキュメント標準の Issue と関連付けます。手動で行う代わりに、すでに接続されている GitHub ツールを使って Copilot に依頼しましょう。Issue を検索し、Issue をクローズする PR を作成します。

> [!NOTE]
> この演習では、Copilot に GitHub 上の操作を初めて依頼します。プロジェクトテンプレートでは、GitHub ツールがすでにワークスペースに接続されています。Copilot が最初にツールを使うとき、VS Code で GitHub へのサインインを求められることがあります。画面の指示に従って認証してください。後の演習では、この接続の仕組みである Model Context Protocol について確認します。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、agent picker で **Agent** が選択されていることを確認します。
3. Instruction file の更新に関する Issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。instruction の変更と、今後のフィルタリング作業に向けた publishers helper の基盤の両方を説明に含めます。

   ```text
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を検索し、PR を作成します。
5. Copilot が GitHub 操作の実行を **Allow** するよう求めたら、コマンドを確認して適切に許可します。
6. PR が作成されたら、次のプロンプトを使って PR をマージし、ローカルのブランチを `main` に戻します。

   ```text
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub 操作とシェルコマンドの実行を **Allow** するよう Copilot に求められたら、内容を確認して適切に許可します。

これで GitHub Copilot の支援を使ってプルリクエストを作成し、マージできました。

## まとめと次のステップ

Copilot がこのプロジェクトの instruction file からコンテキストを取得する仕組みを確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の instruction を使ってコード生成プロンプトを送信し、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` に新しいリポジトリ標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードに新しい標準が反映されることを確認しました。
- instruction の更新と helper の基盤を `main` に commit しました。次の演習でこの状態を引き継ぎます。

次は [agent mode][next-lesson]を使って、Copilot にコードベース全体にまたがる新機能を追加してもらいます。

## リソース

- [GitHub Copilot の custom instruction file][instruction-files]
- [Custom instruction を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot の custom instruction を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用 custom instruction][personal-instructions]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
