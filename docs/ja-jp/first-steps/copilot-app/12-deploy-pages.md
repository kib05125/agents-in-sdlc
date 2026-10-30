---
title: "レッスン 12 - GitHub Pages でサイトをデプロイする"
description: "GitHub Copilot app を使って Pages ワークフローを設定し、そのプルリクエストをレビューして、公開された Space Quiz を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Space Quiz を GitHub Pages にデプロイして、誰でもアクセスできるようにしましょう。Copilot はワークフローを準備し、デプロイを案内できますが、変更をレビューしてサイトが公開されたことを確認するのは自分の役割です。

このレッスンでは、次のことを行います。

- Copilot に GitHub Pages の設定を依頼します。
- マージする前にワークフローとプルリクエストをレビューします。
- デプロイの進行状況を確認し、公開されたサイトを検証します。

## シナリオ

Space Quiz はすでに GitHub リポジトリにありますが、手軽に試す方法がありません。単一ファイルのサイトを GitHub Pages で公開すれば、サーバーや実行時依存関係を追加せずに公開 URL からアクセスできるようになります。

## Copilot にサイトのデプロイを依頼する

`space-quiz` の Copilot app セッションで、次のプロンプトを送信します。

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![マージされた space-quiz のプルリクエスト、Build website・Upload artifact・Deploy to Pages の各ステップが成功した GitHub Actions の Deploy website 実行、および yourname.github.io/space-quiz/ で公開された Space Quiz の 3 段階の図。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> このプロンプトは、エージェントにプルリクエストの作成とマージ、およびリポジトリの Pages 設定の変更を依頼します。続行する前に、差分、デプロイ権限、GitHub の承認要求を確認してください。

## ワークフローをレビューしてデプロイを確認する

1. README は、リポジトリにまだ存在しない場合にのみ追加されていることを確認します。
2. ビルドとデプロイの各ステップを含め、プルリクエストの変更をレビューします。
3. 必須のプルリクエストチェックが成功した後にのみマージします。
4. GitHub Actions のデプロイ実行を追跡し、成功するまで確認します。
5. 報告された URL を開き、Space Quiz が表示されることを確認します。

エージェントが Pages を有効にできない場合やプルリクエストをマージできない場合は、案内された手順に従ってください。デプロイが成功し、公開サイトが読み込まれるまで、完了と判断しないでください。

## まとめと次のステップ

Space Quiz を公開し、エージェントの報告だけに頼らず、サイトが公開されたことを確認できました。続けて[レッスン 13: 振り返りと次のステップ][next-lesson]に進みます。

[next-lesson]: ../13-review/
