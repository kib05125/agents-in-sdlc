---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

Poucas organizações não enfrentam problemas com dívida técnica. Pode ser o caso de problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de funcionalidades que ficaram no backlog por falta de tempo para implementá-las. O agente de nuvem do GitHub Copilot foi criado para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina, ele cria um rascunho de pull request para que uma pessoa desenvolvedora revise o trabalho. Assim, você pode delegar tarefas repetitivas, acelerar o desenvolvimento e se concentrar em questões mais abrangentes.

Neste exercício, você vai explorar:

- como personalizar o ambiente de geração de código.
- como garantir que as operações sejam realizadas com segurança.
- a importância de issues com escopo claro.
- como atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas dívidas técnicas para resolver. As pessoas contratadas inicialmente para criar a primeira versão do site deixaram a documentação em um estado inadequado — na verdade, ela está totalmente ausente. Como primeiro passo, a equipe quer adicionar comentários TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer melhorar a descoberta de jogos. Ela gostaria que a página de detalhes de cada jogo mostrasse jogos relacionados — outros títulos da mesma categoria — para que as pessoas possam continuar explorando. Ainda não é necessário ter uma implementação finalizada; a equipe quer algo que possa usar para testes de aceitação da experiência. Isso é um bloqueio no momento, mas há outras issues de prioridade mais alta.

Esses são exemplos de tarefas que podem ser adiadas com facilidade e são boas candidatas para atribuição ao agente de nuvem do Copilot. Ele pode trabalhar nelas de forma assíncrona enquanto você cuida de outras tarefas. Depois, você revisa o trabalho e confirma que está correto.

## Conheça o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano de forma semelhante a uma pessoa desenvolvedora. Assim como no trabalho em equipe, há várias maneiras de iniciar uma tarefa, incluindo [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois de receber a atribuição, o Copilot cria um rascunho de pull request para acompanhar o progresso, prepara o ambiente e começa a trabalhar. Você pode examinar a sessão enquanto o trabalho está em andamento ou depois de concluído. Quando a solução estiver pronta para revisão, o Copilot mencionará você no pull request.

## A importância de instruções com escopo claro

O GitHub Copilot não faz mágica: você ainda precisa pensar. Mesmo operações aparentemente simples podem se tornar complexas quando examinadas em detalhes. Por isso, [defina com cuidado o escopo das tarefas para o agente de nuvem do Copilot][cloud-agent-best-practices]. Trate-o como uma pessoa parceira de programação com IA: trabalhe por etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a chegada da IA generativa.

## Instruções personalizadas neste repositório

Nos exercícios anteriores, você conheceu as instruções personalizadas e como elas orientam o Copilot. Se já fez o exercício prático sobre instruções personalizadas, viu o arquivo `.github/copilot-instructions.md` em ação. Caso contrário, este é um bom momento para examiná-lo rapidamente. Antes de atribuir trabalho ao agente de nuvem do Copilot, leia os arquivos de instruções incluídos neste repositório para perceber o efeito deles mais adiante.

Abra os arquivos a seguir na interface Web do GitHub ou em um codespace, se já houver um em execução:

- `.github/copilot-instructions.md` — revise a seção **Code standards**, especialmente as convenções de TypeScript e as expectativas para comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que associa essas instruções aos arquivos `**/*.test.ts`.

Ao atribuir a issue `Code lacks documentation` ao agente de nuvem na próxima seção, observe no pull request resultante os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de comentário. Essas orientações vêm dos arquivos de instruções que você acabou de revisar. Voltaremos a esse assunto ao revisar os PRs em um exercício posterior.

## Configure o ambiente de desenvolvimento do agente de nuvem do Copilot

Criar código, independentemente de quem esteja fazendo o trabalho, normalmente exige um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja pronto. O mesmo vale ao atribuir tarefas ao Copilot, que trabalha de forma semelhante a uma pessoa desenvolvedora de software.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizá-lo com um [workflow de configuração especial][setup-workflow], definido em `.github/workflows/copilot-setup-steps.yml`, que é executado antes do início do trabalho. Assim, o agente tem acesso às ferramentas e dependências necessárias. Esse arquivo já foi configurado para facilitar o laboratório e permitir que você conheça esse recurso. Ele disponibiliza Node.js, as dependências do projeto, os navegadores do Playwright para testes de ponta a ponta e o banco de dados SQLite migrado e preenchido com dados para o aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões no nível mínimo necessário para *suas etapas*. O Copilot receberá seu próprio token para as operações dele.
    permissions:
      # Para clonar o repositório durante as etapas de configuração, por exemplo, para instalar dependências, você precisará da permissão `contents: read`. Se não clonar o repositório nessas etapas, o Copilot fará isso automaticamente depois que elas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend/aplicativo - Node.js (todo o aplicativo agora usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migre e preencha o banco de dados SQLite local para que compilações e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

Esse arquivo se parece com qualquer workflow do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`. Esse job é executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- Observe o gatilho `workflow_dispatch`, que permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para verificar se o workflow funciona em vez de esperar que o Copilot o execute.

## Adicione documentação

Embora todo mundo entenda a importância da documentação, a maioria dos projetos tem informações desatualizadas ou não tem documentação alguma. Esse tipo de dívida técnica costuma ficar sem solução, o que reduz a produtividade e dificulta a manutenção da base de código ou a integração de novas pessoas à equipe. O Copilot é especialmente útil para criar documentação, então esta é uma boa issue para atribuir ao agente de nuvem. Ele trabalhará em segundo plano e gerará a documentação necessária. Você voltará para revisar o resultado em um exercício futuro.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para criar uma issue em branco.
5. Defina o **Title** como `Code lacks documentation`.
6. Defina a **Description** como:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) recém-gerado, cujo título será parecido com `[WIP]: Code lacks documentation`. Se nenhum PR novo aparecer na lista, aguarde um ou dois minutos e atualize a janela do navegador.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas pode levar alguns minutos para aparecer no PR. O Copilot está preparando o ambiente (executando o workflow descrito anteriormente), analisando o projeto e definindo a melhor abordagem para resolver o problema.

13. Revise a lista e as tarefas que o agente pretende realizar.
14. Role a linha do tempo do pull request. Você verá uma atualização indicando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

  ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Percorra a sessão em tempo real e observe como o Copilot resolve o problema: ele explora o código e entende o estado atual, pausa para pensar e definir um plano e, então, cria código.

Isso provavelmente levará alguns minutos. Um dos principais objetivos do agente de nuvem do Copilot é executar tarefas de forma assíncrona para que você possa se concentrar em outras atividades. Vamos aproveitar esse recurso atribuindo outra tarefa ao agente e, em seguida, criando uma funcionalidade no aplicativo.

## Adicione uma seção de jogos relacionados à página de detalhes do jogo

Uma das grandes vantagens do agente de nuvem do GitHub Copilot é poder dividir o trabalho: você se concentra em algumas tarefas enquanto ele cuida de outras. Talvez não leve muito tempo para adicionar uma seção de jogos relacionados para a equipe de design, mas esse tempo pode ser usado em outras atividades. Vamos atribuir a tarefa ao agente de nuvem.

1. Volte ao seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para usar o modelo em branco.
5. Defina o **Title** como `Show related games on the game details page`.
6. Defina a **Description** como:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Em pouco tempo, você verá o emoji 👀 no primeiro comentário da issue, indicando que o Copilot está trabalhando nela.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot agora está trabalhando na segunda solicitação. Como o agente de nuvem trabalha de forma semelhante a uma pessoa desenvolvedora, você não precisa acompanhá-lo ativamente; basta revisar o resultado quando estiver pronto. Agora vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Nesta lição, você explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com ele, você pode atribuir issues ao Copilot para execução assíncrona. Use-o para resolver dívidas técnicas, criar funcionalidades ou ajudar a migrar código de uma estrutura para outra.

Você explorou:

- como personalizar o ambiente de geração de código.
- como garantir que as operações sejam realizadas com segurança.
- a importância de issues com escopo claro.
- como atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha em segundo plano, vamos criar e usar agentes personalizados. O [agente de nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e tem acesso a instruções personalizadas, que vimos nos módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuição de issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
