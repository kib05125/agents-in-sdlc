---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルタリング、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストは、すべて 1 つのブランチに保存されています。まとめてレビューし、プルリクエストを作成しましょう。星評価のプルリクエスト (PR) は手動でマージしましたが、今回は **Agent Merge** に進行を任せます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。このワークショップでは、手順を簡略化するため、フィルタリングと品質確認の一連の作業を 1 つの会話とブランチで進め、この PR にまとめています。

このレッスンでは、次のことを行います。

- Agent Merge と、マージまでの作業を自動化する方法を学びます。
- 機能全体の変更と検証結果を調べます。
- フィルタリングの PR を作成します。
- レビュー後にのみ Agent Merge を有効にして、PR がマージされることを確認します。

## シナリオ

フィルタリングの作業を通じて、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys では、PR の残りの作業を自動化したいと考えていますが、マージの許可は開発者が管理します。

## Agent Merge とは

**Agent Merge** は、プルリクエストをマージ可能な状態にするための残りの作業を自動化します。有効にすると、Copilot は PR を妨げている問題 (失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要に応じた rebase など) に取り組みます。その後、リポジトリで許可されている場合は GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せられますが、有効にする前に変更をレビューし、マージしてよいことを確認してください。

## Agent Merge を使って PR を管理する

コードの作成が終わったので、まとめてレビューし、PR を作成して、残りの処理を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. 選択肢から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを確認します。すべてのチェックに成功すると、マージが実行されます。
7. Copilot から「PR #14 was squash-merged successfully.」のようなメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリの設定、権限の不足を回避しません。処理がブロックされた場合は、報告された理由を確認し、リポジトリで許可されている場合はレビュー済みの変更を手動でマージしてください。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストの処理など、開発プロセスの一部を自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge と、マージまでの作業を自動化する方法を学びました。
- 機能全体の変更と検証結果を調べました。
- フィルタリングの PR を作成しました。
- レビュー後にのみ Agent Merge を有効にして、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、省略可能なクラウドへの委任に使える[便利な Copilot CLI の slash command][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
