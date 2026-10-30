---
title: "Lição 1 - Instale o aplicativo GitHub Copilot"
description: "Instale o aplicativo GitHub Copilot, conecte o repositório criado a partir do modelo, conheça o workspace e inicie uma conversa rápida."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

O **[aplicativo GitHub Copilot][about-copilot-app]** é um aplicativo para desktop voltado ao desenvolvimento com agentes. Baseado no GitHub Copilot CLI e integrado nativamente ao GitHub, ele permite usar repositórios, branches e pipelines de CI imediatamente. O aplicativo foi criado para fluxos em que você orienta vários agentes em paralelo — cada um em seu próprio workspace isolado — em vez de fazer todo o trabalho manualmente, além de automatizar tarefas repetitivas. Com o Node.js instalado e a cópia do projeto pronta, o próximo passo é instalar o aplicativo e conectar o repositório.

Nesta lição, você vai:

- instalar o aplicativo GitHub Copilot e entrar na sua conta.
- adicionar ao aplicativo o projeto do repositório GitHub.
- conhecer o workspace, incluindo o backlog criado pelo modelo.
- iniciar uma conversa rápida para saber mais sobre o próprio aplicativo.

## Cenário

Sua equipe está adotando agentes de IA para dar conta de um backlog crescente. O aplicativo Copilot oferece um único lugar para orientar esse trabalho: selecionar issues, executar agentes, revisar alterações e fazer merge de pull requests. Nesta lição, você instalará e conectará o aplicativo e aprenderá a iniciar uma conversa sobre o projeto.

> [!NOTE]
> É necessário ter um plano elegível do Copilot: Copilot Free, Copilot Student ou qualquer plano pago (Pro, Pro+, Business ou Enterprise). Se você usa o Copilot Business ou Enterprise, o administrador precisa manter habilitada a política do **aplicativo GitHub Copilot**.

## Instale e configure o aplicativo GitHub Copilot

Para usar o aplicativo GitHub Copilot, primeiro instale-o. Há versões para Windows, macOS e Linux. Vamos instalar o aplicativo, autenticar a conta e adicionar o repositório Tailspin Toys.

1. Em um navegador, abra a [página inicial do aplicativo GitHub Copilot][download-app].
2. Baixe o aplicativo para sua plataforma e instale-o seguindo as instruções da página.
3. Abra o aplicativo depois da instalação.
4. Selecione **Sign in to GitHub** e siga as instruções para autenticar. Se você usa o GitHub Enterprise Cloud com residência de dados, selecione **Use GitHub Enterprise** e informe o nome do host `*.ghe.com` quando solicitado. O GitHub Enterprise Server não é compatível.
5. Se o processo inicial oferecer a seleção de repositório, selecione o repositório Tailspin Toys criado a partir do modelo. Se preferir, ignore essa etapa e adicione o projeto depois.
6. Quando solicitado, escolha um tema e conclua a configuração inicial.
7. Procure seu repositório em **Sessions**, na barra lateral. Se ele já estiver listado, não é necessário adicioná-lo novamente.
8. Se o repositório ainda não estiver conectado, selecione **+** ao lado de **Sessions** para criar uma sessão e, depois, selecione **Add GitHub repository**. Informe o proprietário e o nome do repositório anotados na lição de pré-requisitos.
9. Confirme se o repositório aparece na barra lateral. No layout usado aqui, pastas de repositórios, como `tailspin-toys`, ficam agrupadas em **Sessions**, com as sessões individuais abaixo de cada repositório.

## Conheça o workspace

Com o projeto conectado, conheça a interface. O aplicativo organiza o trabalho em algumas áreas na barra lateral:

- **New** — permite iniciar uma nova sessão de chat com o Copilot.
- **Pull requests** — exibe seus pull requests pela integração nativa com o GitHub. Você pode navegar e filtrar os PRs, verificar o status de CI e revisá-los sem sair do aplicativo.
- **Issues** — exibe suas issues pela mesma integração. Você pode navegar e filtrar as issues e iniciar uma sessão a partir de uma delas.
- **Automations** — tarefas salvas de agentes que são executadas conforme uma agenda ou sob demanda. Elas ajudam a gerenciar listas de tarefas, manutenção periódica do projeto e outras atividades repetitivas. O resumo apresenta essas automações como próximo passo, não como outro exercício do workshop.
- **Customize** — permite adicionar recursos ao aplicativo Copilot, como servidores MCP, plug-ins, skills e outros componentes. Você usará esta opção para configurar o Playwright MCP.
- **Chats** — conversas rápidas para perguntas e ideias que não precisam de uma branch ou workspace próprio. Esta seção aparece depois que você inicia o primeiro chat. Se ela não estiver visível, selecione **+** ao lado de **Sessions** para iniciar um chat.
- **Sessions** — agrupa pastas de repositório e sessões de agentes. Selecione o nome de um repositório, como `tailspin-toys`, para abrir a exibição do repositório, ou selecione uma sessão abaixo dele para voltar à conversa. Cada sessão tem seu próprio workspace isolado, permitindo executar várias ao mesmo tempo sem misturar as alterações. Você iniciará a primeira sessão ao adicionar avaliações por estrelas.

Ao longo do workshop, você explorará o workspace.

> [!TIP]
> Em caso de dúvida, pergunte ao Copilot. Se não souber como fazer algo ou se não tiver certeza de que é possível, peça orientação ao Copilot.

### Encontre o backlog incluído

Quase todo projeto tem um backlog, e a Tailspin Toys não é exceção. Vamos explorar o backlog criado automaticamente quando você criou o repositório a partir do modelo.

1. Em **Sessions**, na barra lateral, selecione a pasta do repositório Tailspin Toys, e não uma sessão individual abaixo dela. Depois, selecione **Issues** na navegação superior do repositório.
2. Procure estas issues pelo título, em vez de presumir seus números:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecione uma issue para ler os detalhes. Cada issue também pode iniciar uma sessão de agente. Você começará pela issue de filtragem depois de fazer uma pequena alteração inicial.

> [!NOTE]
> As listas de **Issues** e **Pull requests** são filtradas automaticamente para mostrar apenas itens dos repositórios adicionados ao aplicativo Copilot. Para ver itens de outros repositórios, adicione-os ao aplicativo.

## Inicie uma conversa rápida

Uma ótima forma de conhecer o aplicativo é usá-lo para aprender sobre o *próprio aplicativo*. Uma **conversa rápida** é ideal para isso. Você pode fazer uma pergunta ou trocar ideias sem criar uma branch nem um worktree — é uma conversa temporária e não exige uma sessão.

1. Na barra lateral, selecione **+** ao lado de **Chats** para iniciar uma conversa. Se **Chats** ainda não estiver visível, selecione **+** ao lado de **Sessions** para iniciar seu primeiro chat; **Chats** aparecerá depois.
2. Pergunte ao aplicativo como funcionam as sessões:

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. Leia a resposta na exibição da conversa. Você verá que cada sessão é executada em seu próprio worktree Git isolado — isso permite executar vários agentes em paralelo sem misturar as alterações. Você pode continuar a conversa ou iniciar outro chat quando quiser.

## Resumo e próximos passos

Parabéns! Você instalou o aplicativo GitHub Copilot, conectou seu projeto e explorou o workspace. Você aprendeu a:

- instalar o aplicativo e entrar na conta do GitHub.
- adicionar um projeto a partir do repositório GitHub.
- conhecer o workspace e localizar o backlog incluído na guia **Issues**.
- usar uma conversa rápida para fazer uma pergunta temporária.

Em seguida, você [iniciará sua primeira sessão de agente][next-lesson] para exibir uma avaliação por estrelas nos cards de jogos.

## Recursos

- [Sobre o aplicativo GitHub Copilot][about-copilot-app]
- [Introdução ao aplicativo GitHub Copilot][getting-started]
- [Trabalho com sessões de agentes no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
