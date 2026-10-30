---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせた、専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置き、対象タスクに効果的に取り組めるよう、Copilot の指示、ベストプラクティス、コーディングパターン、専門知識を指定します。チームの知識を再利用できるエージェントにまとめられます。たとえば、[WCAG][wcag] への準拠を確認するアクセシビリティエージェント、安全なコーディングに従うセキュリティエージェント、一貫したテストパターンを守るテストエージェントを作成できます。

カスタムエージェントは、プロジェクト内の `.github/agents` または全体で使う `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、そのエージェントの動作、専門知識、指示を記載する Markdown プロンプトが必要です。

### カスタムエージェントと agent skill の比較

カスタムエージェントと [agent skill][agent-skills-concept] には役割の重なりがあります。どちらも主に Markdown ファイルで定義し、AI に作業方法を指示します。違いを簡単に表すと、**カスタムエージェントは作業者で、スキルはツール**です。

カスタムエージェントは独自のコンテキストウィンドウを持ち、作業を進めるためにスキルやほかのエージェントを調整します。このラボのアクセシビリティエージェントは、アクセシビリティのガイドラインに沿ってサイトをレビューし、更新します。その作業の一部として、プルリクエストのワークフロースキルや、テストを実行・管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成方法に唯一の正解はありません。AI のほかの使い方と同様に、環境やシナリオに合わせてテストと調整を行ってください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、誰もがクラウドファンディングプラットフォームを使えるようにしたいと考えています。ユーザーから、テキストと背景色のコントラストが不十分なため、ダークテーマが読みにくいという意見が寄せられています。このアクセシビリティの問題に対処するため、デザインチームはユーザーが切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なため、できるだけ早く実装します。カスタムエージェントを使って機能を生成しましょう。

この演習では、次のことを行います。

- 既存のアクセシビリティ用カスタムエージェントを確認します。
- Copilot Chat でアクセシビリティエージェントを使い、ハイコントラストモードを実装します。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントが用意されています。内容を確認して、Copilot への指示を理解しましょう。

Codespace に戻り、ターミナルを開いて、アクセシビリティの作業用に `main` から新しいブランチを作成します。演習 3 のフィルタリング PR とは分けて管理します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` の YAML frontmatter を確認します。

> [!CAUTION]
> `name` と `description` の frontmatter は、カスタムエージェントに必須です。

3. 続くセクションを確認します。次の内容が記載されています。
   - アクセシブルな Web サイトのコードを生成する際の主な責務。
   - アクセシビリティのベストプラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や間違い。

## Copilot Chat でカスタムエージェントを使う

VS Code の Copilot Chat ビューの下部にあるエージェント選択メニューには、`.github/agents` に定義されたすべてのカスタムエージェントが表示されます。カスタムエージェントを選択すると、その指示とツールに合わせてチャットセッションを設定できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な場合は各手順で説明します。

1. Chat ビューのエージェント選択メニューで、まだ選ばれていない場合は **Agent** を選びます。

   ![Chat ビューのエージェント選択メニューを示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. チャットビュー下部にあるエージェント選択メニューを開きます。ここには現在のエージェントが表示されます。既定では **default** です。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選びます。
4. 次のプロンプトをアクセシビリティエージェントに送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat がコードベースを調べて変更を提案し、プロジェクトファイルに編集を適用します。変更はファイルパスと差分とともにチャットに表示されます。
6. エージェントが変更した作業ファイルの一覧は、**Files changed** の表示に反映されます。

> [!NOTE]
> この作業には数分かかる場合があります。Copilot はリポジトリに変更を加え、Astro コンポーネント、CSS、関連テストなどの既存ファイルを編集します。

次の演習では、進行中の作業をレビューし、方向付けます。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]について学びました。カスタムエージェントは、タスクや分野に特化した AI アシスタントです。チームの専門知識や標準を再利用できるエージェントにまとめ、Copilot の作業をより効果的に導けます。

この演習では、次のことを確認しました。

- カスタムエージェントの定義方法。
- Copilot Chat のエージェントモードでカスタムエージェントを使う方法。

次は[エージェントの作業を監視して方向付け][next-lesson]、変更を確認しながら、同じセッションにライトモードの切り替え機能を追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの設定][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
