---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを利用できるようにします。コンテキストを共有する方法はいくつかあります。その中心となるのが [instruction file][instruction-files] です。コード生成の指針を記述することで、必要なコードだけでなく、その構成方法も Copilot に伝えられます。

この演習では、次のことを行います。

- リポジトリの custom instruction とパス単位の instruction file を通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- *現在の* 指示で、フィルタリングに使う最初のデータスライス (publishers helper) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトをもう一度実行し、生成コードに新しい標準が反映されることを確認します。
- 次の演習で Copilot が更新後のガイダンスを使えるよう、指示の更新とフィルタリングの基礎を `main` に commit します。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot は確率的に動作します。この演習では、指示の更新後に動作がどう変化するかを確認します。出力が文字単位で一致することが目的ではありません。

## Instruction file

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のとおりです。

- データレイヤーには、必ず単体テストを用意します。
- UI はダークモードで、現代的な印象にします。
- コードのドキュメントには TSDoc の doc comment を使います。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

instruction file を使い、Copilot がプラクティスに沿ってタスクを進められるようにします。

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

instruction file の作成に関する詳しい説明は、この演習の範囲外です。サンプルプロジェクトに含まれる例は、一般的な方法を示しています。主なポイントは次のとおりです。

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

まず、このリポジトリに用意されている instruction file を確認します。中核となる `copilot-instructions.md` が 1 つあり、さまざまなタスク向けに `*.instructions.md` ファイルが用意されています。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡潔な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** の指針も確認してください。これらの指示は Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドでは、指示を適用するファイルを決める glob パターン (リポジトリルートからの相対パス) を指定しています。この例では、`**/*.test.ts` に一致する TypeScript のテストファイルが対象です。
5. このプロジェクトで単体テストを作成するときの指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、一番下までスクロールします。`unit-tests.instructions.md` などの instruction file や、プロジェクトの既存ファイルへのリンクがあります。大きな指示を小さく再利用しやすいファイルに分け、コード生成で参考にする例を示せます。ここに記載されたパスはリポジトリルートではなく、instruction file からの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で TSDoc の doc comment とファイルコメントのヘッダーに関するルールを追加します。

## 変更用ブランチを作成する

ベストプラクティスに沿って、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する*前に* Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な場合は各手順で説明します。

Custom instruction の影響を確認するため、まずは現在の指示でプロンプトを送信します。その後、指示を更新し、同じプロンプトを再送信して違いを確認します。

1. 前の演習で開いたエディタータブを閉じ、Copilot に必要なコンテキストだけを渡します。
2. `src/lib/publishers.ts` を開き、helper の配置先を Copilot に示します。
3. Chat ビューのエージェント選択メニューで **Agent** を選び、ファイルを変更できるようにします。

   ![Chat ビューのエージェント選択メニューを示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べ、コードを適用します。helper とテストなど、複数のファイルが変更される場合があります。
6. 提案された helper が型付き関数で、最初の引数に `db` クライアントを受け取り、型付きの publisher 配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約に基づいています。
7. 提案されたコードに TSDoc の doc comment とファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくても doc comment を追加することがあります。その場合も問題ありません。この演習の目的は、指示の更新後に一貫性が向上することを確認することです。

## 新しいリポジトリ標準を追加する

前述のとおり、`.github/copilot-instructions.md` はプロジェクトレベルの情報を Copilot に伝えるファイルです。コード提案の質を高めるため、リポジトリのコーディング標準を記載しましょう。

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

## 同じプロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話をリセットします。
2. `src/lib/publishers.ts` を選択し、Copilot の対象ファイルを指定します。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックがあることを確認します。

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

新しいプロジェクト標準に従うよう Copilot を導き、次の演習で利用する実際のコードに適用できました。

## 指示とブランチを commit して push する

instruction file もリポジトリ内のほかのファイルと同じように、ソース管理で管理します。変更を commit してリポジトリに push しましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押し、新しいターミナルウィンドウを開きます。
2. ターミナルで次を実行し、指示と helper の変更を確認します。

   ```bash
   git status
   ```

3. 指示の更新と helper の基礎を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの関連ファイルが追加されていたら、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリに push します。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチを push したので、バックログのドキュメント標準に関する Issue に関連付けたプルリクエストを作成します。GitHub ツールを使えるよう Copilot に設定されているため、Issue を探し、Issue をクローズする PR を作成してマージするよう依頼できます。

> [!NOTE]
> この演習では、Copilot に GitHub 上の操作を依頼します。プロジェクトのテンプレートでは GitHub ツールがワークスペースに設定済みです。初めて使用するときに、VS Code で GitHub へのサインインを求められる場合があります。画面の案内に従って認証してください。後の演習で、この接続の仕組みである Model Context Protocol について学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** が選ばれていることを確認します。
3. instruction file の更新に関係する Issue を探し、指示の更新と publishers helper の基礎の両方を説明する現在のブランチから PR を作成するよう Copilot に依頼します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成します。
5. Copilot が代理で GitHub 操作を実行する許可を求めたら、コマンドを確認して必要に応じて **Allow** を選択します。
6. PR が作成されたら、次のプロンプトで PR をマージし、ローカルブランチを `main` に戻すよう依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub 操作とシェルコマンドの実行許可を求められたら、コマンドを確認して必要に応じて **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトの instruction file から Copilot がコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の指示でコード生成のプロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` に新しいリポジトリ標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードに新しい標準が反映されることを確認しました。
- 次の演習で基礎として使えるよう、指示の更新と helper を `main` に commit しました。

次は、Copilot がコードベース全体に機能を追加する[エージェントモード][next-lesson]で、指示を活用します。

## リソース

- [GitHub Copilot のカスタマイズ用 instruction file][instruction-files]
- [Custom instruction 作成のベストプラクティス][instructions-best-practices]
- [Copilot 向けの custom instruction をより良く書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用 custom instruction][personal-instructions]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
