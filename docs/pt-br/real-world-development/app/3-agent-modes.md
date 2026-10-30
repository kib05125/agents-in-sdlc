---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Explore os modos de agente: use Plan para definir uma abordagem, Autopilot para implementar filtros a partir de uma issue e Interactive para revisar e verificar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Começamos adicionando uma pequena funcionalidade ao projeto, mas alterações maiores exigem um processo mais robusto. Felizmente, o aplicativo GitHub Copilot foi criado para trabalhar com o fluxo existente da organização e ajudar a construir as soluções certas da maneira correta. Esta é a primeira de várias lições em que você seguirá um processo comum de desenvolvimento com agentes: começará com uma issue para criar uma funcionalidade, verificará o código e o comportamento e, por fim, fará o merge da alteração no projeto.

> [!NOTE]
> Você usará a mesma sessão ao longo do fluxo de desenvolvimento da funcionalidade. Em geral, você usaria sessões ou PRs diferentes para cada tipo de arquivo, mas neste workshop vamos simplificar para focar nos conceitos principais.

Nesta lição, você vai:

- iniciar uma sessão de agente a partir de uma issue do GitHub.
- definir requisitos no modo **Plan**.
- implementar a funcionalidade no modo **Autopilot**.
- revisar o código.
- validar manualmente a funcionalidade em um canvas de navegador.

Durante o desenvolvimento, você atualizará as instruções do repositório, personalizará a skill `quality-checks` existente, adicionará validação com MCP, criará um agente de QA e abrirá o PR da funcionalidade.

## Cenário

O catálogo da Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e editora. A issue do backlog descreve a funcionalidade, mas é preciso decidir detalhes — como a combinação de categorias — antes de escrever o código. Você usará o modo Plan para tomar essas decisões e, depois, autorizará uma implementação com escopo definido pelo Autopilot.

## Contexto

A introdução de agentes de programação com IA ao fluxo de desenvolvimento não muda os fundamentos — na verdade, torna-os ainda mais importantes. A maioria das pessoas desenvolvedoras segue um fluxo semelhante a este:

1. Abrir uma issue registrada com os detalhes do trabalho necessário.
2. Planejar o que precisa ser desenvolvido.
3. Criar e revisar o código.
4. Executar testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar um pull request (PR).
7. Depois da revisão do código e da aprovação do processo de integração contínua, fazer o merge.

> [!NOTE]
> Os detalhes exatos variam conforme a equipe e a organização, mas a maioria dos fluxos segue alguma variação desse processo.

Ao seguir essa abordagem, você garante que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de revisão que o código escrito manualmente.

## Modos de sessão

O **modo de sessão** controla o nível de autonomia do agente. Você pode defini-lo no menu suspenso abaixo do campo de prompt e alterá-lo quando quiser:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria um plano primeiro. Você o revisa e aprova antes da execução.
- **Autopilot**: o agente trabalha de forma totalmente autônoma, escrevendo código, executando testes e iterando sem esperar sua orientação.

Comece no modo Plan, revise o plano e depois use o Autopilot para implementá-lo.

## Inicie uma sessão a partir da issue

Antes de começar, confirme que o PR de avaliação por estrelas foi mesclado e que sua `main` local está atualizada.

1. Em **Sessions**, na barra lateral, selecione a pasta do repositório Tailspin Toys e, na navegação superior, selecione **Issues**. Abra a issue **Allow users to filter games by category and publisher**. Essa issue já está incluída; não é necessário criar outra.
2. Selecione **New session** e escolha uma **new working tree** baseada na `main` atualizada.

    ![Visualização da issue no aplicativo GitHub Copilot com uma seta apontando para o botão New session](../../../_images/app-new-session-from-issue.png)

3. Confirme se a issue está associada à sessão e selecione **Plan** no seletor de modo.

> [!NOTE]
> Se o Node.js ou o npm não forem encontrados, peça ao agente que execute `node --version` e `npm --version`. Se os comandos funcionarem no terminal, mas não na sessão, verifique o `PATH` ou a configuração do gerenciador de versões e feche e reabra o aplicativo completamente. Repita a operação e as verificações com falha; não reinstale o npm apenas porque o agente não o encontrou.

## Planeje a funcionalidade de filtragem

O planejamento permite revisar a abordagem antes de o Copilot escrever código. Como você iniciou pela issue, o Copilot já tem a solicitação como contexto. Envie:

```plaintext
Build this feature.
```

Responda às perguntas do Copilot e compare o plano com os critérios de aceitação da issue. Confirme se ele inclui filtragem por categoria e editora, controles acessíveis, alterações de acesso a dados e testes. Converse sobre qualquer comportamento indefinido, como a combinação de várias categorias ou o resultado quando nenhum jogo corresponde.

O plano deve incluir lint, testes de unidade, testes E2E e verificação de tipos com as ferramentas existentes do projeto. Mantenha o escopo em implementar e testar a filtragem; você criará o PR depois de concluir o fluxo de qualidade. Peça alterações ao plano antes de aprová-lo e guarde a URL da issue e as decisões acordadas para validá-las mais tarde.

## Aprove explicitamente o Autopilot

Quando estiver satisfeito com o plano, selecione **Approve and implement this plan** ou a opção equivalente na sua versão. Confirme qual modo está ativo, sem presumir que a aprovação o alterou. Verifique se o indicador de modo mostra **Autopilot**.

O Copilot começará a trabalhar na implementação, seguindo o plano, gerando código e executando testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto revise o plano primeiro. Se o Copilot informar que faltam dependências ou que há conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas. Encerre somente os servidores que você iniciou.

## Revise e verifique a implementação

Assim como qualquer outro código, o código gerado precisa ser revisado antes do merge. Vamos revisar o código e executar o site para confirmar que tudo funciona.

1. Abra **Changes** e inspecione a implementação dos filtros e os testes.
2. Compare o resultado com a issue e as decisões aprovadas, incluindo combinações de categorias e editoras. Confirme se as alterações seguem as instruções existentes do repositório.
3. Inspecione os resultados de lint, testes de unidade, testes E2E e verificação de tipos. Uma verificação ignorada não foi aprovada.
4. Resolva as falhas e execute novamente as verificações afetadas antes de aceitar a implementação. A configuração E2E do Playwright compila e serve uma prévia e pode reutilizar um servidor local; confirme se o servidor testado pertence a este worktree, e não a uma lição anterior.

## Explore a nova funcionalidade

O código parece correto — mas será que funciona? Vamos iniciar o aplicativo como antes e abrir o site em um canvas de navegador.

1. Use este prompt para pedir ao Copilot que inicie o aplicativo e abra a página no canvas de navegador:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. Em alguns instantes, o aplicativo será iniciado e uma janela do navegador aparecerá dentro do aplicativo Copilot.
3. Selecione uma categoria e confirme se os resultados correspondem a ela. Limpe o filtro e selecione uma editora para verificar os resultados.
4. Combine os filtros de categoria e editora e compare os resultados com a issue e o plano aprovado. Se houver suporte a várias categorias, verifique o comportamento combinado acordado.
5. Teste uma combinação sem jogos correspondentes e confira o estado vazio. Limpe ou redefina os filtros e confirme se a lista completa volta a aparecer.
6. Quando terminar, peça ao Copilot que encerre o servidor de desenvolvimento iniciado para a sessão:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Mantenha esta sessão para as próximas lições

Ainda não crie nem faça merge do PR de filtragem. Mantenha esta sessão, este worktree e esta branch para as Lições 4 a 8. Em seguida, você aplicará instruções personalizadas ao código de filtragem e adicionará melhorias para a skill e o agente de QA antes de criar e mesclar o PR combinado na Lição 8. Depois de encerrar o servidor de desenvolvimento, você pode deixar a sessão inativa e voltar a ela na próxima lição.

## Resumo e próximos passos

Você usou diferentes modos de agente para desenvolver e revisar uma funcionalidade. Nesta lição, você:

- iniciou uma sessão de agente a partir de uma issue do GitHub.
- definiu requisitos no modo **Plan**.
- implementou a funcionalidade no modo **Autopilot**.
- revisou o código.
- validou manualmente a funcionalidade em um canvas de navegador.

Em seguida, vamos entender melhor como o código é gerado e garantir que siga os padrões documentados ao [usar instruções personalizadas][next-lesson].

## Recursos

- [Trabalho com sessões de agentes no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
