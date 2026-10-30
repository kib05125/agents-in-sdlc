---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーでフィルタリング機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書く以外にも、データや外部サービスを使ったり、追加の自動化ツールを Copilot に提供したりする必要があります。MCP サーバーを使うと、CLI に組み込まれている機能を超えて、Copilot にツールやサービスを提供できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) と Copilot CLI での使い方を学びます。
- まだ利用できない場合は、Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作させ、フィルタリング機能を試します。

## シナリオ

単体テストやエンドツーエンドテストは重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。Copilot に、ユーザーと同じように作業中の Web サイトを使わせることで、変更が期待どおりに動作するか、さらに自動で確認できます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントは外部サービスとリアルタイムにやり取りし、最新情報にアクセスしたり、ユーザーに代わって操作を実行したりできます。

これらのツールやリソースには、AI エージェントと外部サービスをつなぐ MCP サーバーを介してアクセスします。各 MCP サーバーは、エージェントが利用できるツールやリソースを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プルリクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザーの自動操作機能を提供します。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステムの発見や貢献を促進するために [MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーは、プロジェクトのほかの依存関係と同様に扱ってください。利用する前にソースコードを確認し、発行元を検証して、セキュリティへの影響を検討してください。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、Copilot がユーザーと同じように Web サイトを操作できるよう追加します。

> [!NOTE]
> Copilot CLI で Playwright MCP をすでに設定している場合は、セットアップを飛ばして次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバーの追加画面を開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類が **STDIO** であることを確認して <kbd>Enter</kbd> を押し、続けて <kbd>Tab</kbd> を押します。
5. **Command** の入力欄に次を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP 画面を終了します。

## Playwright で機能を試すよう Copilot に依頼する

これまでは機能が期待どおりに動くことを手動で確認しました。次は Copilot で利用できる Playwright MCP サーバーを使って、同じ確認を行います。

1. 次のプロンプトで、Playwright MCP サーバーを使って機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常は MCP サーバーを指定しなくても、Copilot が適切に使います。ここでは使うツールを明示して、より一貫した結果を得られるようにしています。

2. Copilot がブラウザーで機能を確認する手順を実行する様子を見ます。
3. レポートを読み、期待どおりに動作していることを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作してからサーバーを停止し、レポートを提示します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使って、実際のブラウザーで機能を確認できました。ここでは、次のことを行いました。

- Model Context Protocol (MCP) と Copilot CLI での使い方を学びました。
- まだ利用できない場合は、Playwright MCP サーバーを追加しました。
- エージェントにブラウザーを操作させ、フィルタリング機能を確認しました。

次は、[QA カスタムエージェントを作成します][next-lesson]。スキルとブラウザーツールを専門的な役割にまとめます。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
