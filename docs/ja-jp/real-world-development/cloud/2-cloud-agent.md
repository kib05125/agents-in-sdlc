---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどありません。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなど、さまざまな問題があります。GitHub Copilot の cloud agent は、コードの更新や機能の追加などのタスクを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を任せて開発を加速し、開発者がより大きな課題に集中できるようにします。

Copilot cloud agent を使って、次のことを学びます。

- コード生成環境をカスタマイズする方法。
- 操作を安全に行う方法。
- 範囲を明確に定めた Issue の重要性。
- Issue を Copilot に割り当てる方法。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。最初のサイトを作成した委託先がドキュメントを適切に整備しなかったため、ドキュメントがほとんどありません。まず、アプリケーションのすべてのエクスポート関数に TSDoc の doc comment を追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリの別のゲームを表示すれば、ユーザーは閲覧を続けられます。まずは、UX の受け入れテストに使える簡単な実装を希望しています。この作業は現在ブロッカーですが、ほかに優先度の高い Issue があります。

どちらも、優先度を下げられやすいタスクです。Copilot cloud agent に割り当てれば、非同期に作業を進めてもらい、ほかのタスクを進めた後に戻って作業をレビューできます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドで作業できます。たとえば、[GitHub Issue を Copilot に割り当てる][assign-issue]方法があります。割り当てると、Copilot は進行状況を追跡するドラフトのプルリクエストを作成し、環境をセットアップしてタスクに取りかかります。作業中でも完了後でも、Copilot のセッションを詳しく確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確に定めた指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単そうな操作でも、詳しく見ると複雑なことがあります。[Copilot cloud agent のタスク範囲を慎重に決めてください][cloud-agent-best-practices]。AI のペアプログラマーとして、段階を踏み、学び、試し、適宜調整します。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリの custom instruction

前の演習では、custom instruction と Copilot への指示方法を学びました。custom instruction の演習をすでに行った場合は、`.github/copilot-instructions.md` が機能することを確認しています。まだの場合は、この機会に簡単に見てみましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリに用意された instruction file を読み取り専用で確認すると、後でその効果を見つけやすくなります。

リポジトリの github.com ページ、または起動済みの Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクション、特に TypeScript の規約と TSDoc doc comment に関するルールを確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` frontmatter を確認します。この指示は `**/*.test.ts` に一致するファイルに適用されます。

次のセクションで **Code lacks documentation** Issue を cloud agent に割り当てます。生成されたプルリクエストに TSDoc doc comment、TypeScript の規約、コメントヘッダーが反映されているか確認してください。後の演習で PR をレビューするときにも、改めて確認します。

## Copilot cloud agent の開発環境をセットアップする

コードを作成するときは、開発環境やセットアップスクリプトが必要になることがあります。Copilot にタスクを割り当てる場合も同じです。Copilot は SWE と同じような方法で作業します。

Cloud agent は [GitHub Actions][github-actions] を使って作業環境を準備します。`.github/workflows/copilot-setup-steps.yml` に[セットアップワークフロー][setup-workflow]を作成すると、作業前に実行される環境をカスタマイズでき、必要な開発ツールや依存関係を使えるようになります。ラボの流れをスムーズにするため、あらかじめ設定してあります。このワークフローでは、Copilot が Node.js、プロジェクトの依存関係、エンドツーエンドテスト用ブラウザー、単一 Astro アプリ用にマイグレーションと seed を実行した SQLite データベースを利用できるようにします。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 手順に必要な最小限のアクセス許可を設定します。Copilot は操作用の独自トークンを受け取ります。
    permissions:
      # 依存関係のインストールなど、セットアップ手順でリポジトリをクローンする場合は `contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL です)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテスト用のデータを用意するため、ローカル SQLite データベースをマイグレーションして seed します。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同じように見えますが、重要な点が 2 つあります。

- `copilot-setup-steps` という単一のジョブがあります。このジョブは、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーにより、リポジトリの **Actions** タブからワークフローを手動で実行できます。Copilot の実行を待たずに、正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、情報が古かったり、まったくなかったりするプロジェクトは少なくありません。こうした技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメント作成が得意なため、cloud agent に割り当てるのに適した Issue です。バックグラウンドで必要なドキュメントを生成します。後の演習で作業をレビューします。

1. 新しいブラウザータブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

   ![Issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

   ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。一覧に PR が表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が todo リストを作成したことを確認します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで数分かかる場合があります。Copilot は環境を作成し (前述のワークフローを実行)、プロジェクトを分析して、問題に取り組む方法を決めています。

13. todo リストと、実行予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッションビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するため、ブラウザーのウィンドウを更新する必要がある場合があります。

16. ライブセッションをスクロールし、Copilot が問題を解決する様子を確認します。コードを調べて状態を理解し、方針を考えて計画を決め、コードを作成します。

完了まで数分かかる場合があります。Copilot cloud agent の主な目的は、作業を非同期で進め、ほかの作業に集中できるようにすることです。この利点を活かすため、Copilot cloud agent にもう 1 つのタスクを割り当て、その間にアプリケーションへの機能追加を始めます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の 1 つは、作業を分担できることです。デザインチーム向けに関連ゲームを追加する作業は長時間かからないかもしれませんが、その時間を別の作業に使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。
9. **Assign** を選択します。

まもなく、Issue の最初のコメントに 👀 が表示され、Copilot が作業を始めたことが分かります。

![Copilot が Issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent は SWE と同様に作業するため、常に監視する必要はありません。完了後にレビューできます。次は、カスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を使い、Copilot に Issue を割り当てて非同期に作業させる方法を学びました。cloud agent を使うと、技術的負債の解消、新しい機能の作成、別のフレームワークからのコード移行などを進められます。

このレッスンでは、次のことを確認しました。

- コード生成環境をカスタマイズする方法。
- 操作を安全に行う方法。
- 範囲を明確に定めた Issue の重要性。
- Issue を Copilot に割り当てる方法。

cloud agent がバックグラウンドで作業している間に、次はカスタムエージェントを作成して使います。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで学んだ custom instruction も使用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
