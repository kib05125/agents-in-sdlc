---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを利用できるようにします。[Instruction file][instruction-files] では、必要なコードだけでなく、その構成方法も Copilot に伝えられます。

この演習では、次のことを行います。

- リポジトリの custom instruction とパス単位の instruction file を通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習では変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub Issue 上で非同期に作業するため、リアルタイムに並べて効果を示すことが難しいためです。このハーネスの後半で、cloud agent が作成したプルリクエストをレビューするときに、instruction file の効果を確認できます。

## Instruction file

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のとおりです。

- データレイヤーには、必ず単体テストを用意します。
- UI はダークモードで、現代的な印象にします。
- コードのドキュメントには TSDoc の doc comment を使います。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

instruction file を使い、Copilot がプラクティスに沿ってタスクを実行できるようにします。

### Custom instruction

Custom instruction を使うと、コンテキストや好みを Copilot に伝え、コーディングスタイルや要件を理解させられます。コーディング規約、ライブラリ、追加したいコメントの種類などを指定できます。リポジトリ全体にも、特定のファイル種別にも指示を適用できます。

instruction file には 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリの**すべて**のリクエストで Copilot に送られる単一の instruction file です。技術スタック、作成中のものの概要、ベストプラクティスなど、ほとんどのチャットや CLI のリクエストに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別向けに作成できます。TypeScript や Astro などの言語、UI コンポーネントの作成、単体テストの追加などに関する指針を提供できます。

> [!NOTE]
> IDE では、instruction file は Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの instruction file と `applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使います。
>
> また、Copilot は [ほかの標準形式の instruction file][custom-instructions-support] (AGENTS.md や CLAUDE.md など) もサポートしています。

### Instruction file を管理するためのベストプラクティス

instruction file の作成に関する詳しい説明は、このワークショップの範囲外です。サンプルプロジェクトに含まれる例は、一般的な方法を示しています。主なポイントは次のとおりです。

- `copilot-instructions.md` には、作成中のもの、プロジェクト構造、全体的なコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` ファイルでは、ファイル種別 (単体テスト、Astro コンポーネント、データレイヤーなど) や特定のタスクに関する指示を提供します。
- 自然な言葉で、明確なガイダンスを書きます。コードの良い例と避けるべき例も示します。

instruction file の作成方法も、AI の使い方も 1 つではありません。試しながら、プロジェクトに最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した instruction file を用意します。このプロジェクトでは、[UI の更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク向けのファイルを確認できます。
>
> Copilot に instruction file を生成させることもできます。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、各環境での操作方法は異なります。関連する演習で手順を説明します。
>
> テンプレートや出発点を探す場合は、instruction file、カスタムエージェントなどのリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトの custom instruction file を確認する

このリポジトリに用意されている instruction file を確認します。中核となる `copilot-instructions.md` が 1 つあり、さまざまなタスク向けの `*.instructions.md` ファイルが用意されています。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡潔な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** の指針も確認してください。これらの指示は Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドでは、指示を適用するファイルを決める glob パターン (リポジトリルートからの相対パス) を指定しています。この例では、`**/*.test.ts` に一致する TypeScript のテストファイルが対象です。
5. このプロジェクトで単体テストを作成するときの指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、一番下までスクロールします。`unit-tests.instructions.md` などの instruction file や、プロジェクト内の既存ファイルへのリンクがあります。大きな指示を小さく再利用しやすいファイルに分け、コード生成で参考にする例を示せます。ここに記載されたパスはリポジトリルートではなく、instruction file からの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で TSDoc の doc comment とファイルコメントのヘッダーに関するルールを追加します。

## 新しいリポジトリ標準を追加する

この演習で編集するのは、TSDoc の doc comment とファイルレベルのコメントヘッダーを使ってコード内にドキュメントを記載する、プロジェクト全体のルールです。後の演習で Issue を割り当てると、cloud agent はこのルールを参照します。

編集の前にブランチを作成します。cloud agent は Issue の対象ブランチから指示を読み取りますが、ここでは変更を整理して commit します。

1. Codespace のターミナルで新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供するファイルです。コード提案の質を高めるため、リポジトリのコーディング標準を記載します。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. **Code formatting requirements** セクションを探します。27 行目付近にあります。コーディング標準は記載されていますが、コード内ドキュメントのルールがありません。そのため、生成された helper に doc comment が含まれませんでした。
3. 既存の標準の直後に、次の Markdown 行を追加し、ファイルコメントのヘッダーと TSDoc の doc comment を Copilot に要求します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで学んだように、instruction file は全体的なガイダンスを提供するリポジトリレベル (`.github/copilot-instructions.md`) にも、特定の言語、ファイル種別、タスク向けの `*.instructions.md` にも作成できます。今回追加した doc comment のようなプロジェクト全体の標準には、リポジトリレベルのファイルが適しています。

## 指示の更新を commit、push してマージする

Cloud agent は、Issue の対象ブランチから instruction file を読み込みます。次の演習で Copilot に Issue を割り当てると、`main` からブランチが作成されます。cloud agent が更新した指示を使えるよう、変更を `main` にマージする必要があります。

1. 変更を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチを push します。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを作成してマージします。次の演習で Issue を割り当てると、cloud agent はこの指示を読み込みます。

> [!TIP]
> ワークショップで `main` に直接作業する場合は、ブランチを省略して `main` に commit できます。ブランチを使う手順は、実際のプロジェクトでの進め方に合わせています。

## まとめと次のステップ

Copilot がこのプロジェクトの instruction file からコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。このハーネスの後半で、cloud agent が作成したプルリクエストを確認し、その標準が適用されていることを確認します。

次は、[Copilot cloud agent に最初の Issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズ用 instruction file][instruction-files]
- [Custom instruction 作成のベストプラクティス][instructions-best-practices]
- [Copilot 向けの custom instruction をより良く書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
