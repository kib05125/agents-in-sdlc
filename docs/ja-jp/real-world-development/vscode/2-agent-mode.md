---
title: "演習 2 - Copilot エージェントモードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、複数のファイルを変更し、テストを実行する必要があることがよくあります。開発者は通常、必要なファイルを探して変更し、テストを実行して、デバッグし、変更し忘れたファイルを見つけて、さらに修正するという流れで作業します。こうした作業は次々に発生します。

このような場面で役立つのが Copilot エージェントモードです。

Copilot エージェントモードは、IDE 内でより自律的に作業します。開発者と同様に、既存のプロジェクト構造を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。エージェントモードを使って、サイトに新しい機能を追加してみましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は用途が異なります。エージェントモードは IDE 内で作業し、すばやいフィードバックと対話を可能にします。Cloud agent はチームメンバーのように非同期で作業し、Issue やプルリクエストを通じてやり取りします。

この演習では、次のことを学びます。

- Copilot エージェントモードがプロジェクトを調べ、関連ファイルを特定し、複数の変更を調整する方法。
- GitHub Copilot エージェントモードが UI とデータレイヤーにまたがる機能を実装する方法。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリや発行元で絞り込めるようにします。前の演習では publishers helper を追加しました。次は Copilot エージェントモードを使って、データレイヤー、UI、テストの残りを実装します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを見て、現在の機能を確認します。

この Web サイトは、開発者向けテーマのボードゲームを扱うクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示します。Astro アプリケーションはビルド時に静的 HTML を生成します。ページは frontmatter 内で Drizzle ORM を使ってローカルの SQLite データベースを直接参照します。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセス helper は `src/lib/` に置き、インタラクティブな機能は標準 DOM API を使う小さな Astro `<script>` で追加します。

### Web サイトを起動する

サイトを簡単に起動できるよう、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習を終えていれば、`main` に戻っているはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルウィンドウを開きます。
3. フィルタリング作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くかどうか尋ねるダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して、Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは、Web サーバーをホストしている Codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトを起動したら、機能を確認します。主な機能は次のとおりです。

- **ホームページ**: タイトル、画像、説明を含むボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどを含む詳細が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な場合は各手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加して改善しましょう。まずバックログを確認します。テンプレートからプロジェクトをセットアップすると、Issue のバックログが自動的に作成されています。GitHub Copilot に項目を表示するよう依頼します。

1. Chat ビューのエージェント選択メニューで **Agent** を選びます。**Agent** はファイルを調べて複数の変更を計画・実装し、ターミナルコマンドやツールを実行します。

   ![Chat ビューのエージェント選択メニューを示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選びます。

> [!CAUTION]
> ワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップの作成時には Claude Sonnet 4.5 を使ったため、手順にも記載しています。提案の一貫性を高めることが目的ですが、LLM は確率的に動作するため、提案が記載内容と異なる場合があります。これは通常の動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の MCP コマンドを使う場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信して、バックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選んで、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot は、ユーザーに最も役立つと考えられる項目に優先順位も付けています。

## Instruction file を確認する

エージェントにコードを生成させる前に、作業のコンテキストを Copilot に伝える instruction file を確認します。Web サイトへの機能追加の進め方を説明する [user interface (UI)][ui-instructions] ファイルを使います。

1. Codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加に関する全体的なガイダンスを確認します。内容には次の項目が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle など、ファイル種別に応じた instruction file へのリンク。

> [!TIP]
> instruction file から、ほかの instruction file やプロジェクト内のファイルを参照できます。パスは instruction file からの相対パスです。指示を再利用しやすく、小さく分割して、例やテンプレートを示せます。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## フィルタリング機能を実装する

フィルタリング機能を完成させるには、少なくとも次の 3 つの更新が必要です。

- データレイヤー (`src/lib/`) に残りのフィルタリングロジックを追加または調整します。
- フィルタリングの動作を確認するテストを追加または更新します。
- ゲーム一覧ページにフィルタリング UI を追加します。

コードベースにマージする前に、テストを実行して成功することも必要です。Copilot エージェントモードにこれらの作業を任せましょう。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択して instruction file を追加します。

   ![UI instruction file を選択する例を示すスクリーンショット。](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント選択メニューで **Agent** が選ばれていることを確認します。

   ![Chat ビューのエージェント選択メニューを示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. モデルが引き続き **Claude Sonnet 4.5** であることを確認します。
5. 次のプロンプトで、バックログの関連 Issue に基づいて機能を実装するよう Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、必要な機能に関連するファイルを探してから、データレイヤー、UI、テストを変更して実行する様子を確認します。

   ![Copilot がプロジェクトのファイルを調べるスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトを調べ、ファイルを変更し、テストを実行します。タスクやコードベースの複雑さによっては、数分かかることがあります。作業中、エディターに **Keep** と **Undo** が表示される場合があります。作業が完了すると、変更ごとに **Keep** または **Undo** を選べるため、進行中に選択する必要はありません。

7. Copilot から求められたら、**Continue** を選んでテストを実行します。

   ![Copilot Chat ペインでテスト実行への同意を求めるダイアログのスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。Copilot はタスクを完了し、エラーを検出しなくなるまでコード生成とテストを繰り返します。

   ![Copilot エージェントモードの会話が完了した画面のスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot やほかの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを起動したブラウザーに戻り、新しい機能を試します。
11. 動作を確認してコードをレビューしたら、Copilot Chat ウィンドウの **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot エージェントモードを使って、Tailspin Toys の Web サイトに機能を追加しました。ここでは、次のことを学びました。

- GitHub Copilot エージェントモードで UI とデータレイヤーにまたがる機能を実装する方法。
- Copilot エージェントモードでプロジェクトを調べ、関連ファイルを特定し、複数の変更を調整する方法。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューする方法。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加の演習: ページネーションを実装する

ゲーム一覧が増えると、ページネーションが必要になります。この演習で学んだ方法を使い、Copilot にページネーションを実装するよう依頼します。次の点を検討してください。

- 既存の instruction file を使うなど、現在のベストプラクティスに従います。
- ページサイズをユーザーが選べるようにするか、固定値にするかなど、ページネーションの動作を決めます。
- 希望する実装に必要なガイダンスをプロンプトに含めます。
- Copilot に変更を依頼したりコンテキストを追加したりして、何度か調整することもあります。これは Copilot を使う通常の流れです。

## リソース

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
