---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用できるカスタマイズ、関連リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys で一連の作業を進めました。具体的には、次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、用意されているフィルタリングの Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリングの Issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- custom instruction でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを確認しました。
- 品質保証 (QA) のカスタムエージェントを作成して選び、要件、テスト範囲、スキルの結果、ブラウザーでの検証内容を評価しました。
- フィルタリングの変更全体をレビューし、Agent Merge にフィルタリングの PR のマージを許可しました。
- コンテキスト、モデル、共有、省略可能なクラウドへの委任に使う slash command を確認しました。

## 作成したもの

ワークショップには、それぞれ更新済みの `main` から作成したブランチで進める 2 つの PR があります。

1. **星評価:** 既存の `starRating` と、評価がない場合の状態をゲームカードに表示します。
2. **フィルタリングと品質ワークフロー:** フィルタリングを実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

フィルタリングの計画から PR 作成まで、同じ会話とブランチを使いました。ワークショップを簡略化するため、その作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を通じた Copilot のブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` スキルは単体テスト、lint、型チェックを実行し、カスタマイズした形式で報告します。QA エージェントは、その結果に要件とテスト範囲のレビューを加えて確認します。

追加するテストは、実際に不足しているテスト範囲を補うものである必要があります。追加テストが不要と判断される QA 実行も適切な場合があります。マージを許可する前に、コードと検証結果を確認してください。変更を加えたら、影響を受ける検証結果を更新します。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業の結果を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。こうしたカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。指示は標準を定め、スキルは繰り返し実行するタスクをまとめ、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。**Plan** は実装前に方針を検討するとき、**Interactive** は小さな変更を進めながら状況を把握したいとき、**Autopilot** は範囲が明確なタスクに使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

開発環境と同様に、コンテキストも重要です。何を作るのか、なぜ必要なのか、どう実現したいのかを明確に伝えると、出力は大きく変わります。

## さらに試す

主要なワークフローを学びました。次の CLI 機能も確認してみてください。

- `/review` でコードレビューエージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を相談し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して実行します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

どのツールも、使い続けることで活用方法が上達します。実際のコード、趣味のコード、長い間作りたかった小さなアプリなどで試してみてください。学んだことをチームで共有し、チームの知見からも学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、省略可能な [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境も確認する場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を参照してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のカスタマイズ][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
