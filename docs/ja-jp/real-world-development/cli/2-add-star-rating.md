---
title: "レッスン 2 - 星評価を追加する: 小さく始める"
description: "Copilot CLI でゲームカードに小さな変更を加え、転送されたブラウザーで確認し、最初のプルリクエストをマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールし、会話を試しました。次はプロジェクトに最初の変更を加えます。小さな変更から始めましょう。ゲームのデータには星評価がすでにありますが、ホームページのゲームカードにはまだ表示されていません。エージェントに評価を表示させ、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次のことを行います。

- 機能ブランチで Copilot との会話を開始します。
- エージェントに小さな変更を依頼します。
- `/diff` で変更をレビューします。
- 転送されたブラウザーでアプリを実行し、変更を確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。一方、ホームページのゲームカードにはタイトル、カテゴリ、発行元、説明しか表示されていません。ウォームアップとして、各カードに既存の評価を表示するようエージェントに依頼します。最初のセッションにぴったりな、小さく独立した変更です。

## 会話のしくみ

**会話**では、タスクについて Copilot CLI とやり取りします。Copilot app と異なり、通常の CLI 会話では専用の worktree を作らず、ターミナルで現在チェックアウトしているリポジトリと Git ブランチを使います。保存した会話から同じやり取りを再開できますが、ファイルとブランチはディスク上の通常の Git 状態として残ります。

会話では、プロンプトとエージェントの応答、ファイルを調べて編集するエージェントのツール操作、`/diff` で確認できる変更の 3 つを確認できます。

## 会話を開始して変更を依頼する

機能の実装に向けて、新しい会話を始めましょう。

1. Codespace に戻ります。
2. まだターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. まだ起動していない場合は、次のコマンドで Copilot を起動します。

   ```bash
   copilot --yolo
   ```

4. slash command `/new` を入力して <kbd>Enter</kbd> を押し、新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調べ、ゲーム詳細の表示に使われるファイルを見つけて、必要なコードを作成します。Copilot CLI で新機能を追加できました。

## 差分をレビューする

小さな変更でも、AI が生成した変更はすべてマージ前にレビューします。Copilot CLI で変更を確認しましょう。

1. `/diff` を入力し、変更されたすべてのファイルを調べます。
2. 評価がある場合は数値で、`starRating` が `null` の場合は `No rating yet` と表示されることを確認します。
3. どちらの状態もテストされていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を終了します。

> [!NOTE]
> Copilot を含む生成 AI ツールは決定論的ではなく確率的に動作するため、生成されるコードは異なる場合があります。特定の実装と一致することではなく、動作を確認してください。

## 変更を確認する

コードを読むだけで動作すると判断せず、Copilot に Web サイトを起動させ、Codespaces から転送されたブラウザーで更新後のユーザーインターフェイス (UI) を確認します。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. ゲームカードに 5 段階評価が表示されていることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプルリクエストを作成してマージする

機能を作成できました。次は、コードをプロジェクトにマージするプルリクエスト (PR) を作成します。

1. 既定のエージェントに変更の commit を依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は PR の作成時に既存の commit を push し、PR の URL を表示します。
3. Copilot CLI が表示した URL を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して PR を開きます。
4. 変更されたファイルとチェック結果を確認します。
5. 準備ができたら **Merge pull request** を選び、マージを確定します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

GitHub Copilot CLI を使って、最初の変更をリリースできました。具体的には、次のことを行いました。

- 機能ブランチで Copilot との会話を開始しました。
- エージェントにゲームカードの小さな変更を依頼しました。
- `/diff` で変更をレビューしました。
- 転送されたブラウザーでアプリを実行し、星評価を確認しました。
- 最初のプルリクエストを作成してマージしました。

次は、より大きな機能を作るため、[フィルタリングの Issue から始め、Plan と Autopilot モードを使います][next-lesson]。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
