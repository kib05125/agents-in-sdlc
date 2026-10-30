---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更の計画、コードの作成、動作のテスト、プルリクエストのレビュー、エージェントを使った作業管理を反復して行います。

## シナリオ

あなたは、開発者向けテーマのボードゲームをクラウドファンディングで支援する架空の会社、Tailspin Toys に新しく加わった開発者です。チームのバックログは GitHub Issue として登録されており、すぐに着手できます。フィルタリングやページネーションなどの機能開発に加え、アクセシビリティやコーディング規約など品質を改善する作業も含まれています。

## 作業環境を選ぶ

GitHub Copilot は、さまざまな作業環境で利用できます。自分に合った環境を選んでください。各ワークショップでは、共通の Tailspin Toys シナリオを使い、それぞれの環境に合わせたセットアップから始めます。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、指示、スキル、カスタムエージェント、Playwright MCP、Agent Merge、便利な slash command を試します。

### [GitHub Copilot app][app]

デスクトップアプリでエージェントセッションを並行して実行し、セッションモードを切り替え、Canvas で共同作業し、GitHub Issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub Issue を非同期エージェントに割り当て、作業を進めるための指示を出し、進行状況を監視して、作成されたプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに、Copilot Chat のエージェントモード、MCP サーバー、カスタムエージェントを活用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
