---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で方針を決め、Autopilot で Issue に基づくフィルタリングを実装し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

まずは小さな機能をプロジェクトに追加しました。より大きな変更には、もっとしっかりしたプロセスが必要です。GitHub Copilot CLI は組織の既存のワークフローに沿って作業できるため、適切な方法で適切なものを構築できます。このレッスンから始まる一連のレッスンでは、Issue を基に機能を作り、コードと機能の動作を検証し、プロジェクトへのマージを目指す、一般的なエージェント駆動の開発プロセスを体験します。

> [!NOTE]
> 機能のワークフローを続ける間、同じ会話とブランチを使います。通常はファイル種別ごとにブランチや PR を分けますが、ここでは主な概念に集中するため簡略化します。

このレッスンでは、次のことを行います。

- GitHub Issue から Copilot との会話を開始します。
- Plan モードで要件を定義します。
- Autopilot モードで新しい機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動で検証します。

この機能の作業を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能の PR を作成します。

## シナリオ

Tailspin Toys のカタログが増えているため、訪問者がカテゴリや発行元でゲームを絞り込めるようにします。バックログの Issue には機能が記載されていますが、カテゴリを組み合わせる方法など、コーディング前に合意が必要な詳細があります。Plan モードで決定し、範囲を定めた実装を Autopilot に任せます。

## 背景

AI コーディングエージェントを開発ワークフローに取り入れても、基本は変わりません。むしろ、基本がより重要になります。多くの開発者は、次のような流れで作業します。

1. 実装内容を記述した Issue から始めます。
2. 実装する内容の計画を立てます。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新しい機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 詳細はチームや組織によって異なりますが、ほとんどの場合は上記の流れに沿っています。

この標準的な進め方に従えば、AI が生成したコードが要件を満たし、手作業で書いたコードと同じレビューを受けるようにできます。

## 会話モード

**会話モード**は、エージェントにどれだけの自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押してモードを切り替えます。

- **Interactive**: エージェントと一緒に作業します。エージェントは変更を提案し、次に進む前に入力を待ちます。
- **Plan**: エージェントが最初に計画を作成します。プロジェクトファイルの編集はできません。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行し、タスクの完了まで反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## Issue から始める

フィルタリングの作業を始める前に、Codespace に戻り、リポジトリとターミナルを準備します。

1. Codespace に戻ります。停止している場合は、再起動してから続けます。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新して、フィルタリング作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7` (または同様の番号) で始まることを確認します。`#` を使うと、GitHub 上の Issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルタリング機能を計画する

計画を立てることで、Copilot に作業を任せる前に機能の実装方法やタスクの進め方を決められます。複雑な作業では、少し時間をかけて計画するのが効果的です。Plan モードに切り替え、Copilot に計画を作らせましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** であることを確認します。
2. 前の手順で追加した Issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、最適な方法を検討して計画を作成します。

3. フィルタリングの動作について質問された場合は、希望に応じて回答します。正解は 1 つではありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を上から下まで確認します。
6. 決定した内容と合わない部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューできました。次は Autopilot を使って Copilot に実装を任せましょう。

Autopilot を使うと、Copilot はタスクが完了したと判断するまで問題に取り組みます。

1. **Accept plan and build on autopilot (recommended)** またはインストール済みのバージョンで同等の選択肢を選びます。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装が始まる場合があります。先に計画をレビューしてください。依存関係が不足している場合やポートが競合している場合は、チェック完了と判断する前にセットアップの問題を解決します。

## 実装をレビューして検証する

他のコードと同様に、生成されたコードもマージ前にレビューします。コードを確認し、サイトを起動して問題がないか調べましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示に **Plan** または **Autopilot** が表示されていないことを確認します。
2. `/diff` を入力し、フィルタリングの実装とテストを調べます。
3. 結果を Issue と計画時の決定に照らし合わせます。
4. レビューが終わったら <kbd>Esc</kbd> を押して差分表示を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新しい機能を試す

コードは問題なさそうですが、実際に動くでしょうか。前と同じようにアプリを起動し、Codespaces の転送ポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. 計画で決めたカテゴリと発行元のフィルタリング、およびその組み合わせを試します。
4. リセット時と結果が空の場合の動作が、Issue と決定内容に合っていることを確認します。
5. Codespace に戻り、起動した開発サーバーを停止するよう Copilot に依頼します。

## まとめと次のステップ

会話モードを使い分けて、機能を構築し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub Issue から Copilot との会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動で検証しました。

次は、[カスタム指示を使って][next-lesson]コードを文書化されたプラクティスに沿わせます。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
