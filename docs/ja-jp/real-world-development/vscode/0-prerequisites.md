---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の演習を始める前に、必要なものを準備します。Tailspin Toys リポジトリのコピーを作成し、作業用の [codespace][codespaces] を起動します。また、エディターで GitHub Copilot Chat が使えることを確認します。

## ラボ用リポジトリをセットアップする

作成するコードのために、[テンプレート][template-repository]からリポジトリのインスタンスを作成します。新しいリポジトリにはラボに必要なファイルがすべて含まれます。演習中はこのリポジトリを使います。

1. 新しいブラウザーウィンドウで [Tailspin Toys テンプレート][tailspin-template]に移動します。
2. ラボのリポジトリページで **Use this template** ボタンを選択し、**Create a new repository** を選択して、自分用のコピーを作成します。

    ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの案内に従います。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。

    ![リポジトリテンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. ラボの後半で使うため、作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えておきます。

> [!NOTE]
> **バックログの準備は完了しています**
>
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログも自動的に作成されます。ワークショップではこの Issue を使います。自分で Issue を登録する必要はありません。

## Codespace を作成する

次に、ラボの演習を行う Codespace を作成します。

[GitHub Codespaces][codespaces] は、ブラウザーで直接コードを書き、実行し、デバッグできるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールをサポートする、すべての機能を備えた IDE を利用できます。

1. 作成したリポジトリに移動します。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタンを選択する画面](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選び、**+** ボタンを選択して Codespace を作成します。

    ![新しい Codespace を作成する画面](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、必要なサービスを手動でインストールするよりもずっと早く準備できます。その間に GitHub Copilot の機能を確認してみましょう。

> [!CAUTION]
> 今後の演習で Codespace に戻ります。ひとまず、ブラウザーのタブで開いたままにしてください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers] で動作します。どちらの環境にも、スムーズに進めるために必要な前提条件がインストールされています。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、プロンプトが表示されたら **Reopen in Container** を選択します。VS Code が Codespace と同じ dev container を構築します。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## GitHub Copilot Chat とエージェントモードを使う

GitHub Copilot Chat のエージェントモードを使うには、IDE に GitHub Copilot Chat 拡張機能がインストールされている必要があります。GitHub Codespace を使っている場合は、通常インストール済みです。

> [!TIP]
> GitHub Copilot Chat 拡張機能がインストールされていない場合は、[Visual Studio Code Marketplace からインストール][copilot-chat-extension]できます。または Visual Studio Code で Extensions ビューを開き、**GitHub Copilot Chat** を検索して **Install** を選択します。

拡張機能をインストールした後、GitHub アカウントでの認証が必要になる場合があります。

1. Codespace に戻ります。
2. エディターの右側に Copilot Chat が表示されていない場合は、Codespace の上部にある **Copilot Chat** アイコンを選択します。
3. Copilot Chat ウィンドウに「Hello world」のようなメッセージを入力して Enter キーを押します。Copilot Chat が起動します。
4. 認証されていない場合は、GitHub アカウントへのサインインが求められます。画面の案内に従って認証します。

    ![Copilot Chat の認証プロンプトの例](../../../_images/ex1-copilot-authentication.png)

5. 認証が完了すると、Copilot Chat ウィンドウが表示されます。

## まとめ

ラボ用リポジトリのコピーを作成し、コードを書くときに使う Codespace の作成を開始しました。

## 次のステップ

Copilot に作業を依頼しましょう。プロジェクトの規約を伝える[演習 1 - カスタム指示][next-lesson]に進みます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces を使い始める][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
