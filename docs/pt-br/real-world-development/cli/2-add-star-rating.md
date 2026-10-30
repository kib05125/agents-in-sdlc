---
title: "Lição 2 - Adicione avaliações por estrelas: uma vitória rápida"
description: "Use o Copilot CLI para fazer uma pequena alteração nos cards de jogos, revise-a em um navegador encaminhado e faça o merge do primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou o Copilot CLI e iniciou uma conversa, é hora de fazer sua primeira alteração no projeto. A tarefa será pequena: os dados dos jogos já incluem uma avaliação por estrelas, mas os cards da página inicial ainda não a mostram. Você pedirá ao agente que exiba essa avaliação, revisará a alteração e fará o merge no seu primeiro pull request.

Nesta lição, você vai:

- iniciar uma conversa focada do Copilot em uma branch de funcionalidade.
- pedir ao agente que faça uma pequena alteração no projeto.
- revisar a alteração com `/diff`.
- executar o aplicativo e confirmar a alteração em um navegador encaminhado.
- abrir e fazer o merge do seu primeiro pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma avaliação por estrelas, que já aparece na página de detalhes. Os cards de jogos na página inicial, porém, mostram apenas o título, a categoria, a editora e a descrição. Como exercício inicial, você pedirá ao agente que mostre a avaliação existente em cada card — uma alteração pequena e independente, ideal para sua primeira sessão.

## Estrutura de uma conversa

Uma **conversa** é onde você trabalha com o Copilot CLI em uma tarefa. Diferentemente do aplicativo Copilot, uma conversa normal da CLI usa o repositório e a branch atualmente selecionados no terminal, em vez de criar um worktree dedicado. As conversas salvas permitem retomar a mesma discussão mais tarde, enquanto os arquivos e a branch continuam sendo o estado comum do Git no disco.

Em uma conversa, você verá três elementos: seus prompts e as respostas do agente, a atividade das ferramentas que ele usa para explorar e editar arquivos e as alterações que você pode inspecionar com `/diff`.

## Inicie uma conversa e solicite a alteração

Vamos iniciar uma nova conversa para implementar a funcionalidade.

1. Volte ao Codespace.
2. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Se o Copilot ainda não estiver em execução, use este comando para iniciá-lo:

   ```bash
   copilot --yolo
   ```

4. Inicie uma nova sessão com o comando slash `/new` e pressione <kbd>Enter</kbd>.
5. Use este prompt para solicitar a alteração:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

O Copilot explorará o projeto, localizará os arquivos usados para exibir os detalhes dos jogos e criará o código necessário. Você acabou de adicionar uma funcionalidade com o Copilot CLI.

## Revise o diff

Todas as alterações geradas por IA precisam ser revisadas antes do merge, mesmo as pequenas. Vamos examinar as alterações diretamente no Copilot CLI.

1. Digite `/diff` e inspecione cada arquivo alterado.
2. Confirme que o card do jogo exibe a avaliação numérica quando ela está disponível e `No rating yet` quando `starRating` é `null`.
3. Confirme que os testes abrangem os dois casos.
4. Revise os resultados das verificações executadas pelo Copilot e peça que ele corrija eventuais falhas.
5. Ao concluir a revisão, pressione <kbd>Esc</kbd> para sair da tela de diff.

> [!NOTE]
> Como todas as ferramentas de IA generativa, o Copilot é probabilístico, e não determinístico. O código exato pode variar. Revise o comportamento em vez de esperar uma implementação específica.

## Verifique as alterações

Não basta ler o código e presumir que ele funciona. Vamos pedir ao Copilot que inicie o site para que possamos examinar a interface atualizada no navegador encaminhado pelo Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme que os cards de jogos exibem as avaliações em uma escala de cinco estrelas.
4. Volte ao Copilot e peça que encerre o servidor iniciado:

   ```plaintext
   Stop the development server you started.
   ```

## Abra e faça o merge do primeiro pull request

Você criou a funcionalidade. Agora abra um pull request (PR) para incorporar o novo código ao projeto.

1. Peça ao agente padrão que faça commit da alteração:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Digite `/pr create`. Ao criar o PR, o Copilot CLI enviará o commit existente e exibirá a URL do PR.
3. Para abrir o PR, mantenha pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecione a URL exibida pelo Copilot CLI.
4. Revise os arquivos alterados e as verificações.
5. Quando estiver tudo pronto, selecione **Merge pull request** e confirme o merge.
6. Volte ao Codespace e saia do Copilot CLI com `/exit`.
7. Atualize sua `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximos passos

Parabéns! Você entregou sua primeira alteração usando o GitHub Copilot CLI. Para isso, você:

- iniciou uma conversa focada do Copilot em uma branch de funcionalidade.
- orientou o agente a fazer uma pequena alteração nos cards de jogos.
- revisou a alteração com `/diff`.
- executou o aplicativo e confirmou a avaliação por estrelas em um navegador encaminhado.
- abriu e fez o merge do seu primeiro pull request.

Em seguida, você [começará pela issue de filtragem e usará os modos Plan e Autopilot][next-lesson] para criar uma funcionalidade maior.

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
