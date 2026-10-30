---
title: "演習 4 - エージェントを監視して管理する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの 2 つの演習では、ユーザーエクスペリエンスを改善し機能を追加する、3 つの別々のタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクの進行状況を監視することも重要です。

GitHub.com の[エージェントページ][agents-page]など、cloud agent に割り当てたタスクを管理するツールがいくつもあります。この mission control では、オープンなプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。実行した操作を調べたり、進行中のセッションに追加の指示を出して方向付けたりすることもできます。

このレッスンでは、次のことを行います。

- エージェントページで cloud agent のタスクを監視します。
- 進行中のセッションに指示を出して、機能を追加で依頼します。

## シナリオ

ハイコントラストモードの作成をエージェントに割り当てた後、チームはライトモードも追加するとよいと気付きました。サイトのスタイルを更新し、切り替え機能を追加しているため、一緒に実装するのが自然です。ハイコントラストに加えてライトモードも追加するよう、エージェントの作業を方向付けます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの状態を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左ペインの両方に表示されるタスクの一覧を確認します。次のようなタスクが表示されます。
   - コードベースのドキュメントの更新。
   - ゲームを変更する API の生成。
   - Web サイトへのハイコントラストモードの追加。
3. 実行中のタスクを選択し、Copilot が行った作業を確認します。次のような操作が含まれます。
   - リポジトリからコードをチェックアウトする。
   - Copilot が作業する環境を作成する。
   - MCP サーバーを設定する。
   - 割り当てられたタスクを完了するための各種手順を実行する。

> [!NOTE]
> 表示される具体的な手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。ここでは、PR と変更されたファイルを確認して、さらに作業を監視できます。

## Cloud agent に追加の指示を出す

実行中のタスクを確認したので、Copilot がハイコントラストモードに取り組んでいる間に、ライトモードの切り替えも追加するよう依頼します。

1. ハイコントラストモードを追加するセッションを選択します。表示されるタイトルは Copilot が付けた名前や作業状況によって異なります。

    ![Mission control のアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. 数分間セッションを確認し、セットアップが完了して作業が始まったことを示すメッセージが表示されるまで待ちます。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot が作業中のセッションを方向付けるダイアログを強調表示した、エージェントページのタスク画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを認識し、作業計画に取り入れることを確認します。

## Copilot の作業を待つ

更新したタスクに Copilot が取り組みます。メッセージを送信した時点で進行中だった手順を終えた後、新しい依頼を作業に取り入れます。

完了まで数分かかります。ここで一度休憩して、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

GitHub Copilot cloud agent のタスクを監視して導くための中心的な場所である、Copilot agents ページを確認しました。この mission control では、実行中と完了済みのタスクを追跡し、作業内容をレビューして、進行中のタスクの範囲を変更したり追加の指示を与えたりできます。

このレッスンでは、次のことを行いました。

- mission control とエージェントページで cloud agent のタスクを監視しました。
- 進行中のセッションに追加機能を依頼し、作業の方向を変えました。

Copilot がアクセシビリティ機能を実装している間に、次は[Copilot が作成したプルリクエストを改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
