---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメントの標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを進める必要がある場合、そのガイダンスを Copilot が利用できるようにします。[Instruction file][instruction-files] では、必要なコードだけでなく、その構成方法も説明できます。フィルタリング機能を構築したので、Copilot が参照した指示を確認し、ドキュメントの標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパス単位の instruction file がエージェントに適用される方法を確認します。
- コーディング標準を守れるよう、指示ファイルを更新します。
- instruction file がコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のとおりです。

- コメントでは、コードの言い換えではなく、意図や自明ではない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数では、用途、パラメーター、戻り値を TSDoc/JSDoc で文書化します。`db` を引数として注入できる場合は、その引数についても記述します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を文書化し、関連するコードを変更した場合はコメントも最新に保ちます。
- 既存の書式設定と lint のガイダンスを維持します。

instruction file を使い、Copilot がこうしたプラクティスに沿ってタスクを実行するための情報を提供します。

## Instruction file

Custom instruction を使うと、コンテキストや好みを Copilot に伝え、コーディングスタイルや要件を理解させられます。コーディング規約、ライブラリ、追加したいコメントの種類などを指定できます。リポジトリ全体にも、特定のファイル種別にも指示を適用できます。

instruction file には 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリのすべてのリクエストで Copilot に送られる単一の instruction file です。多くのリクエストに関係する技術スタック、プロジェクトの概要、ベストプラクティスなど、プロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスク向けのガイドラインを記載します。

> [!NOTE]
> ほかの instruction 形式やサポート状況は環境によって異なります。特定の形式を前提にする前に、[custom instructions のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトの custom instruction file を確認する

作業を始めやすいよう、サンプルプロジェクトには instruction file が用意されています。変更の影響を確認する前に、既存のファイルを見てみましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディングガイダンスを確認します。この指示は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドで、適用対象のファイルを指定する glob を設定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの instruction file やプロジェクト内のファイルへのリンクを確認します。大きな指示を小さく再利用しやすいファイルに分け、コード生成時に従う例を Copilot に示せます。

## チームのガイダンスに合わせて instruction file を更新する

既存のファイルはよい出発点ですが、改善の余地があります。中核となる `copilot-instructions.md` を変更し、新しく生成される TypeScript に TSDoc コメントが含まれるようにします。

1. `.github/copilot-instructions.md` で、**Code formatting guidance** というセクションを探します。35 行目付近にあります。
2. 次の行を最後の箇条書きとして追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。指示を保存したら、フィルタリングの会話を終了して再開します。これにより、機能のコンテキストを保ったまま、新しいガイダンスを読み込めます。

1. フィルタリングの会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチでリポジトリのルートから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示されたら、フィルタリングの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot が instruction file からコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリの指示とパス単位の instruction file がエージェントに適用される方法を確認しました。
- 指示ファイルを更新して、コーディング標準を守れるようにしました。
- instruction file がコードに与える影響を確認しました。

次は、[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストを一貫して実行できるようにします。

## リソース

- [Copilot CLI に custom instruction を追加する][instruction-files]
- [Custom instruction のサポート][custom-instructions-support]
- [Custom instruction 作成のベストプラクティス][instructions-best-practices]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
