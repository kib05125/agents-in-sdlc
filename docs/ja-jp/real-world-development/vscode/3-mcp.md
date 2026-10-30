---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードを使ってフィルタリング機能を作成しました。プルリクエストを開く前に、ブラウザーで実際に動くことを確認します。アプリを手動で操作する代わりに、**Playwright MCP サーバー**を接続して、Copilot に実際のブラウザーで機能をテストさせます。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- ブラウザーでフィルタリング機能を手動テストするよう Copilot に依頼します。
- ブランチを公開し、フィルタリングのプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外部に接続できると、さらに便利になります。Copilot が外部のツールやサービスと通信するための標準的な仕組みが Model Context Protocol (MCP) です。

![エージェントモードの内部構造と、コンテキスト、LLM、ツール (MCP サーバーや VS Code 拡張機能が提供するツールを含む) との連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部のツールやサービスと通信できます。MCP により、AI エージェントは外部のツールやサービスとリアルタイムにやり取りし、リソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わる操作を実行したりできます。

ツールやリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを介してアクセスします。MCP サーバーは、既存の API や npm パッケージのようなローカルツールなど、AI エージェントと外部ツールとの通信を管理します。各 MCP サーバーは、エージェントが利用できるツールやリソースを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。AI エージェントは、新しいリポジトリの作成、既存リポジトリの更新、Issue やプルリクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザーの自動操作機能を提供します。AI エージェントは、Web ページへの移動、フォームの入力、ボタンの選択などを実行できます。

さまざまなツールやリソースにアクセスできる MCP サーバーがほかにもあります。GitHub は、エコシステム内での発見や貢献を促進するために [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティを考慮し、MCP サーバーはプロジェクト内のほかの依存関係と同様に扱ってください。利用前にソースコードをよく確認し、発行元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセス許可には注意してください。

## MCP の設定を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで利用する MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに設定されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` の設定はプロジェクトテンプレートに含まれているため、前の演習では Copilot がバックログの Issue を読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS Code は HTTP で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次は 2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと Copilot がブラウザーを操作できるため、機能のテストに役立ちます。

1. `.vscode/mcp.json` の `github` の隣に `playwright` のエントリを追加し、ファイルが次のようになるようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートサーバーの `github` と異なり、`playwright` は**ローカル**サーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを指定すると、ブラウザーを画面に表示せずに実行します。デスクトップ環境がない Codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトはエンドツーエンドテストで Playwright を使うため、通常は Playwright 用のブラウザーもインストール済みです。Copilot がブラウザーがないと報告した場合は、`npx playwright install chromium` を実行するよう依頼し、もう一度試してください。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて起動**します。Copilot がサーバーのツールを初めて必要としたときに、VS Code がサーバーを起動し、信頼するかどうかを尋ねます。

1. `.vscode/mcp.json` で `playwright` のエントリの上に **Start** が表示されます。これを選択してサーバーを起動し、設定が正しいことを確認します。
2. VS Code でサーバーを信頼するか尋ねられた場合は、設定を確認して、サーバーが起動できるよう信頼する選択肢を選びます。

手動での起動は省略できます。実行しない場合は、次の手順で Copilot が初めてツールを使うときに VS Code がサーバーを起動し、信頼を求めます。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。アプリを自分で操作する代わりに、エージェントがページを開き、移動し、フィルターを適用して、その結果を読み取って要約します。会話を離れずに機能を確認する最も簡単な方法です。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティツリー][playwright-mcp-server]を使います。ボタン、リンク、リスト項目など、ラベル付きの構造化された要素を基にエージェントが判断します。支援技術と同じようにページを認識するため、簡単な機能確認がアクセシビリティの基本確認にもなります。

サーバーを接続してアプリを起動したら、次のプロンプトを Copilot に送信して、作成したフィルタリング機能を試します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各手順を実行して結果を報告します。要約を Issue の受け入れ条件と照らし合わせます。問題がある場合は、プルリクエストを作成する前に追加の質問をするか、コードの修正を依頼します。

> [!NOTE]
> このテストには、`http://localhost:4321` でアプリが起動している必要があります。開発サーバーを停止した場合は、プロンプトを送る前にもう一度起動します。Copilot が Playwright MCP サーバーを初めて使うときは、ブラウザーをダウンロードする場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` の実行を依頼して、もう一度試します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認したので、チームにレビューしてもらうプルリクエスト (PR) を作成します。最初に `filtering-vscode` ブランチを公開します。

1. Codespace で **Source Control** パネルに移動し、Copilot が加えた変更を確認します。
2. **+** アイコンを選択して変更を stage します。
3. **Sparkle** ボタンで commit メッセージを生成します。

   ![変更内容を表示する Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリに push します。

## プルリクエストを作成する

github.com や GitHub コマンドラインインターフェイス (CLI) など、プルリクエストを作成する方法はいくつかあります。GitHub Copilot を使っているので、PR の作成も依頼しましょう。Copilot は関連する Issue を探し、PR を Issue に関連付けられます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. エージェント選択メニューで **Agent** が選ばれていることを確認します。GitHub ツールを使用するために必要です。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選び、Copilot が情報を収集して操作を実行することを許可します。初めて GitHub ツールを使うときに GitHub 認証を求められた場合は、画面の案内に従います。
5. Copilot が Issue を検索し、該当するものを見つけて PR を作成することを確認します。
6. Copilot が作成したリンクを選択して PR をレビューします。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する仕組みを学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- 出荷前にブラウザーでフィルタリング機能を手動テストしました。
- ブランチを公開し、フィルタリングのプルリクエストを作成しました。

MCP サーバーを使って機能を*テスト*しましたが、MCP は Copilot を拡張する方法の 1 つです。次は、アクセシビリティレビューなど、対象を絞ったタスクを効率化する[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP レジストリ][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
