---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI のモード、カスタマイズ、MCP ツール、プルリクエストの自動化を学びながら、Tailspin Toys に 2 つの変更を加え、検証してリリースします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** を使うと、ターミナルで GitHub Copilot をエージェント型コーディングアシスタントとして利用できます。コードベースを調べ、コードを生成し、コマンドを実行し、外部ツールに接続できます。すべてコマンドラインから行えるため、グラフィカルエディターに切り替えずに作業を続けられます。

このワークショップでは、Tailspin Toys で一連の作業を進めます。

1. GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして使い方を確認します。
2. 星評価を表示する小さな変更を加え、ブラウザーで確認し、最初のプルリクエスト (PR) を手動でマージします。
3. フィルタリングの Issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューします。
4. リポジトリの指示を更新し、フィルタリングの作業に適用します。
5. 既存の `quality-checks` スキルをカスタマイズし、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーでフィルタリングを確認します。
7. QA カスタムエージェントを作成して使い、要件、テスト範囲、検証の証拠を確認します。
8. フィルタリングの変更全体をレビューし、Agent Merge を使ってフィルタリングの PR をマージします。
9. コンテキスト、モデル、共有、省略可能なクラウドへの委任に役立つ slash command を確認します。

ワークショップでは、星評価と、指示の更新、スキルの更新、QA プロファイル、テストを含むフィルタリングの 2 つの PR を作成します。フィルタリングと品質確認の作業では、同じ会話とブランチを使います。各ツールを試しながら、それまでの作業を引き継げます。

## レッスン

| レッスン | トピック | 説明 |
| ------ | ----- | ----------- |
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成します |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証し、使い方を確認します |
| [2. 星評価を追加する: 小さく始める][ex2] | 最初の変更 | 既存の評価と null の場合の表示を追加し、最初の PR をマージします |
| [3. エージェントモード: Plan と Autopilot][ex3] | エージェントモード | Issue から機能を計画し、Autopilot で構築して、Interactive モードでレビューします |
| [4. カスタム指示で Copilot を導く][ex4] | コンテキスト | 指示を確認・更新し、フィルタリングに適用します |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 繰り返し可能なチェック | 既存のスキルを確認し、レポート形式を変更して実行します |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの確認 | CLI で MCP を設定し、フィルタリングの動作を確認します |
| [7. QA エージェントを作成して使う][ex7] | 要件とテスト範囲 | 専門エージェントを作成して選び、最終的な検証の証拠を集めます |
| [8. 機能の PR を作成してマージする][ex8] | レビューとマージ | 変更全体をレビューし、PR を作成して Agent Merge を使います |
| [9. GitHub Copilot CLI の slash command][ex9] | CLI 機能 | コンテキスト、モデル、共有、省略可能なクラウドエージェントへの委任を確認します |
| [10. 振り返りと次のステップ][ex10] | まとめ | ワークフロー、再利用できるカスタマイズ、関連リソースを確認します |
| [省略可能: Foundry を組み込む][foundry] | ホスト型エージェント | モデルを準備し、カタログに基づくエージェントをデプロイして Web サイトに接続します |

## 前提条件

このワークショップに参加する前に、次を準備してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかのプランがある GitHub アカウント
- [ ] リポジトリと Codespace を作成する権限
- [ ] ターミナルまたはコマンドライン操作の基本的な知識

> [!TIP]
> 有料プランをお持ちでない場合、認証済みの学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれます。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合は、管理者が Copilot CLI を有効にしていることを確認してください。

## はじめる

**[前提条件から始める →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
