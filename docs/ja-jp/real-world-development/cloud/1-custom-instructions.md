---
title: "演習 1 - Custom instruction (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにする必要があります。[instruction file][instruction-files]を使うと、実行してほしいことだけでなく、その方法も Copilot に伝えられます。

この演習では、次のことを行います。

- リポジトリの custom instruction とパス単位で適用される instruction file を通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここではプロンプトの実行前後を比較しません。Copilot cloud agent は GitHub issue を使って非同期に動作するため、リアルタイムで並べて効果を示すのは困難です。このハーネスの後半で、cloud agent が作成したプルリクエストをレビューするときに、instruction file の影響を確認します。

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

## リポジトリ全体に適用する標準を追加する

この演習で編集するのは、コード内のドキュメントを TSDoc の doc comment とファイルレベルのコメントヘッダーとして記載するという、プロジェクト全体のルールです。以降の演習で cloud agent に割り当てるタスクに、このルールが適用されます。

編集を始める前に、作業用のブランチを作成します。cloud agent は issue の対象ブランチにある instruction を読み取りますが、ここでは変更をきちんと commit します。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## Instruction の更新を commit、push、merge する

Cloud agent は、issue の対象ブランチにある instruction file を読み取ります。次の演習で Copilot に issue を割り当てると、cloud agent は `main` からブランチを作成します。そのため、cloud agent が instruction を使えるように、変更を `main` に取り込む必要があります。

1. 変更を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチを push します。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成し、merge します。次の演習で issue を割り当てるとき、cloud agent はこの instruction を読み取ります。

> [!TIP]
> このワークショップでは、`main` に直接作業してもかまいません。その場合はブランチの作成を省略し、`main` に直接 commit してください。ブランチを使う手順は、実際のプロジェクトでの作業方法を再現しています。

## まとめと次のステップ

Copilot がこのプロジェクトの instruction file からコンテキストを取得する仕組みを確認し、`.github/copilot-instructions.md` に新しいリポジトリ標準を追加しました。以降のハーネスで cloud agent が作成するプルリクエストをレビューするときに、この標準が使われていることを確認します。

次は、[Copilot cloud agent に初めての Issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot の custom instruction file][instruction-files]
- [Custom instruction を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot の custom instruction を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
