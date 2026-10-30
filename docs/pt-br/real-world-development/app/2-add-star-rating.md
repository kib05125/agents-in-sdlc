---
title: "Lição 2 - Adicione avaliações por estrelas: uma vitória rápida"
description: "Inicie sua primeira sessão de agente no aplicativo GitHub Copilot, faça uma pequena alteração nos cards de jogos e conclua seu primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Agora que você explorou o workspace e iniciou uma conversa rápida, é hora de começar uma **sessão de agente** e fazer sua primeira alteração no projeto. A tarefa será pequena: os dados dos jogos já incluem uma avaliação por estrelas, mas os cards da página inicial ainda não a mostram. Você pedirá ao agente que exiba essa avaliação, revisará a alteração e fará o merge no seu primeiro pull request.

Nesta lição, você vai:

- iniciar uma sessão de agente e conhecer sua estrutura.
- pedir ao agente que faça uma pequena alteração focada no projeto.
- revisar a alteração na exibição de diff do workspace.
- executar o aplicativo localmente e confirmar a alteração no navegador.
- abrir e fazer o merge do primeiro pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma avaliação por estrelas, que já aparece na página de detalhes. Os cards de jogos na página inicial, porém, mostram apenas o título, a categoria, a editora e a descrição. Como exercício inicial, você pedirá ao agente que mostre a avaliação existente em cada card — uma alteração pequena e independente, ideal para sua primeira sessão.

## Estrutura de uma sessão

Uma **sessão** é uma conversa com um agente executada em seu próprio workspace isolado. Cada sessão tem um **worktree Git e uma branch dedicados**, o que permite executar várias sessões ao mesmo tempo — uma para adicionar uma funcionalidade, outra para corrigir um bug — sem misturar as alterações. As sessões aparecem agrupadas por repositório na barra lateral; selecione uma para alternar para ela.

Em uma sessão, você verá três elementos: a **conversa** com o agente, a **atividade das ferramentas** usadas para explorar e editar arquivos e a lista de **arquivos alterados** com seus diffs.

## Inicie uma sessão e solicite a alteração

Vamos iniciar uma nova sessão para explorar o projeto e implementar a funcionalidade. Durante a [configuração do aplicativo][prior-lesson], você adicionou o projeto a partir do repositório GitHub. Agora, criaremos uma sessão para esse repositório.

1. Abra ou volte ao aplicativo GitHub Copilot.
2. Na parte superior da barra lateral, selecione **+** para iniciar uma sessão.
3. Selecione `tailspin-toys` como repositório.
4. Abaixo da caixa de prompt, selecione uma **new working tree** e o modo **Interactive**. Use este prompt:

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. Pressione <kbd>Enter</kbd> para enviar o prompt ao Copilot.

O aplicativo Copilot começa criando um novo worktree, uma cópia isolada do projeto. Em seguida, explora o projeto, localiza os arquivos que precisam de atualização e cria o código necessário. Você acabou de adicionar uma funcionalidade com o aplicativo Copilot.

## Revise o diff

Todas as alterações geradas por IA precisam ser revisadas antes do merge, mesmo as pequenas. Vamos examinar as alterações no próprio aplicativo Copilot.

1. No canto superior direito do aplicativo, selecione **Toggle review panel**. No painel que abrir, selecione **Changes** para ver os arquivos alterados e seus diffs.

    ![Barra de ferramentas superior do aplicativo GitHub Copilot com uma seta apontando para o botão Toggle review panel à direita de Create PR](../../../_images/app-2-review-panel.png)

2. Você deverá ver código adicionado a `GameCard.astro`, o arquivo principal usado para exibir os detalhes dos jogos. Ele será semelhante ao exemplo a seguir: um bloco pequeno que mostra a avaliação quando disponível e usa `No rating yet` quando `starRating` é `null`.

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Como todas as ferramentas de IA generativa, o Copilot é probabilístico, e não determinístico. O código exato pode variar, mas deve ser relativamente parecido.

## Verifique as alterações

Antes de abrir o navegador, revise os resultados das verificações automatizadas do agente. Confirme se os testes cobrem um `starRating` numérico e o caso alternativo `null`. Uma verificação ignorada ou bloqueada por um pré-requisito ausente não foi aprovada; revise qualquer solicitação de instalação antes de autorizá-la.

Não basta ler o código e presumir que ele funciona. Vamos pedir ao Copilot que abra o site para examinar a interface atualizada em um canvas de navegador.

> [!TIP]
> Um canvas é um widget interativo disponível dentro do aplicativo Copilot. Mais adiante, você explorará canvases personalizados e criará o seu próprio, mas agora usaremos o canvas de navegador integrado.

1. Use este prompt para pedir ao Copilot que inicie o aplicativo e abra a página no canvas de navegador:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. Em alguns instantes, o aplicativo será iniciado e uma janela do navegador aparecerá dentro do aplicativo Copilot.
3. Confirme se os cards dos jogos avaliados mostram o valor em uma escala de cinco estrelas.
4. Quando terminar, peça ao Copilot que encerre o servidor de desenvolvimento iniciado para a sessão:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Abra e faça o merge do primeiro pull request

Você criou a funcionalidade. Agora abra um pull request (PR) para incorporar o código à base existente.

1. No canto superior direito, selecione **Create PR**.
2. Se solicitado, selecione **Sign in with your browser** e siga as instruções para autenticar.
3. Se o Copilot explicar que a política do repositório exige uma issue correspondente antes de criar o PR, confirme que ele deve seguir esse processo.
4. O Copilot criará o PR.
5. Selecione o indicador **PR** acima do chat para abrir o pull request no painel de revisão.
6. Quando estiver tudo pronto, selecione **Ready to merge**.
7. Na nova janela, selecione **Merge pull request** para fazer o merge.

## Resumo e próximos passos

Parabéns! Você entregou sua primeira alteração usando o aplicativo GitHub Copilot. Em especial, você:

- iniciou uma sessão de agente e conheceu sua estrutura.
- orientou o agente a fazer uma pequena alteração focada nos cards de jogos.
- revisou a alteração na exibição de diff do workspace.
- executou o aplicativo localmente e confirmou a avaliação por estrelas no navegador.
- abriu o PR 1, revisou as verificações e fez explicitamente o merge.

Em seguida, você [começará pela issue de filtragem e usará os modos Plan e Autopilot][next-lesson] para criar uma funcionalidade maior.

## Recursos

- [Trabalho com sessões de agentes no aplicativo GitHub Copilot][agent-sessions]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]
- [Gerenciar issues e pull requests com o aplicativo GitHub Copilot][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#instale-e-configure-o-aplicativo-github-copilot
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
