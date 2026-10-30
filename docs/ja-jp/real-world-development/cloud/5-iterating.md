---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザーエクスペリエンスを改善し機能を追加する複数のタスクで、GitHub Copilot を使いました。コードへのドキュメント追加、デザインチームが試せる関連ゲーム機能の作成、ハイコントラストとライトモードの切り替えを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送り、作業を改善しましょう。

### シナリオ

生成 AI を利用しても、ソフトウェア設計や DevOps の基本は変わりません。生成コードは常にレビューし、通常の DevOps プロセスに沿って作業します。チームメンバーがレビューできるようにする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した内容を確認します。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期に、監督なしでタスクを実行するため、安全を確保するためのセキュリティ上の制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みアクセスは作業に使うブランチ**のみ**に制限されます。
- Cloud agent は GitHub Actions 内で動作し、作業用の独立した一時環境を作成します。
- GitHub Actions のワークフローを実行するには、人間による承認が必要です。
- MCP サーバーなど、[外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR)、つまりコードへのドキュメント追加を確認します。github.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR に GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。MCP サーバーなど、外部リソースへのアクセスは既定で制限されているため、これは**想定どおり**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** などのタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot が作業中の場合、PR のタイトルに **[WIP]** が付いています。その場合は作業が完了するまで待ちます。数分かかることがあります。休憩を取ったり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選び、変更を確認します。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく更新されたコードを確認します。TSDoc の doc comment などのドキュメントが追加されています。変更内容は状況によって異なります。

    変更を確認しながら、演習 2 の最初に見た custom instruction file による TSDoc の doc comment、TypeScript の規約、コメントヘッダーを確認します。

6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行するボタン](../../../_images/shared-approve-workflows.png)

9. PR のチェックセクションでワークフローがキューに追加されたことを確認します。正常に進めば、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業することは、一方的なやり取りではありません。チームメンバーと同じように、PR のコメントやコードへのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、対応する新しいセッションを開始します。結果は確率的に変わるため、具体的なプロンプトを指定することはできません。たとえば、次のような更新を依頼できます。

- 各コードファイルの先頭に、役割を説明する短いコメントヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc の doc comment を追加する。
- Astro アプリの構造を説明する README を作成する。

1. 上記の例またはコードベースに追加したいドキュメントについて、**@copilot** をメンションして生成されたドキュメントの変更をコメントで依頼します。
2. **View Session** を選択し、Copilot の作業を確認します。Copilot が更新用の新しいセッションを開始することに注目してください。
3. **Back to pull request** を選択すると PR に戻れます。

    ![プルリクエストに戻るボタン](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、PR に新しい commit が追加されます。
5. **Files changed** タブを選択し、変更をレビューします。

納得できるまで、Copilot に変更を依頼できます。問題がなければ、ドラフトの PR をレビュー可能な状態にして、`main` ブランチにマージします。

![PR をレビュー可能な状態にする操作](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue を解決するため、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルが付いた PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す表示を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行するボタン](../../../_images/shared-approve-workflows.png)

8. PR のチェックセクションでワークフローがキューに追加されたことを確認します。正常に進めば、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
9. **省略可能:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えます。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

関連ゲーム機能ができました。前と同じように、Copilot cloud agent に追加の変更を依頼できます。たとえば、表示する関連ゲームの数を調整したり、コメントヘッダーや TSDoc の doc comment を追加したりできます (この Issue は custom instruction を更新する**前**に割り当てたことを思い出してください)。**Conversation** タブでコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタムエージェントで実装した機能をレビューします。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 の mission control で追加を依頼したライトモードが含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot が作業中の場合、PR のタイトルに **[WIP]** が付いています。数分かかる場合がありますが、作業が完了するまで待ちます。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装をレビューするときは、特に次の点を確認します。
   - モードを切り替える UI コンポーネント。
   - ユーザー設定を保持する local storage の使用。
   - ハイコントラストモードとライトモードの CSS またはスタイルの変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行するボタン](../../../_images/shared-approve-workflows.png)

9. PR のチェックセクションでワークフローがキューに追加されたことを確認します。正常に進めば、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
10. **省略可能:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名に置き換えます。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイコントラストとライトモードの切り替えをテストします。ページを再読み込みしても設定が保持されることを確認してください。

アクセシビリティ用カスタムエージェントが、アクセシビリティのベストプラクティスに沿った機能実装を Copilot に促すことを確認できました。アクセシビリティに関する懸念や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 省略可能な演習 — 委任を続ける

Cloud agent は、バックログの実際の Issue を任せて、その間に別の作業を進めるときに力を発揮します。練習として、リポジトリに Issue をいくつか作成し、Copilot に割り当ててみてください。たとえば、次のような作業があります。

- ゲーム詳細ページに支援者向けの問い合わせフォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセス helper に入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。各レッスンを通じて、次のことを行いました。

- このリポジトリに用意された custom instruction file を確認し、後で cloud agent の出力にどう反映されるかを見ました。
- Copilot cloud agent に Issue を割り当て、環境のセットアップ、計画、非同期の実行を確認しました。
- アクセシビリティ用カスタムエージェントを作成して使い、ハイコントラストとライトモードの切り替えを追加しました。
- Copilot agents ページを mission control として使い、アクセシビリティのセッションを監視して作業中に方向付けました。
- cloud agent のプルリクエストをレビューして改善し、`@copilot` に変更を依頼し、ワークフローを承認しました。

## 振り返りと次のステップ

Cloud agent ハーネスを完了しました。引き続き学ぶ場合は、ほかのハーネスで今回の内容をさらに練習できます。

- **[VS Code ハーネス](../../vscode/)** — IDE から直接、Copilot Chat のエージェントモードと MCP の連携を試します。
- **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、agent skill、QA カスタムエージェント、Playwright MCP、Agent Merge、省略可能な `/delegate` による cloud agent への連携を使い、ターミナルからレビュー済みの変更を 2 つリリースします。

自分のリポジトリで、次のことも試してみてください。

- リファクタリングやテスト範囲の Issue を cloud agent に割り当てる。
- 別の分野向けのカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
