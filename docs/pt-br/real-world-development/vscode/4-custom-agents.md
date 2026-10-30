---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados em tarefas ou domínios específicos do fluxo de desenvolvimento. Ao definir agentes com arquivos Markdown na pasta `.github/agents` do repositório, você fornece instruções direcionadas, práticas recomendadas, padrões de código e conhecimento do domínio para que o Copilot realize determinados tipos de trabalho com mais eficiência. As equipes podem transformar sua experiência em agentes reutilizáveis — por exemplo, um agente de acessibilidade que aplica as diretrizes [WCAG][wcag], um agente de segurança que segue práticas de código seguro ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos os campos `name` e `description`, seguidos de um prompt Markdown que define o comportamento, a especialidade e as instruções do agente.

### Agentes personalizados e agent skills

Há alguma sobreposição entre agentes personalizados e [agent skills][agent-skills-concept]. Ambos são definidos principalmente em arquivos Markdown e orientam uma IA sobre como realizar operações. A distinção mais simples é: um **agente personalizado** é quem executa o trabalho; as **skills** são as ferramentas.

Os agentes personalizados têm sua própria janela de contexto e são criados para orquestrar skills (e até outros agentes) durante o trabalho. Neste laboratório, o agente personalizado de acessibilidade revisará e atualizará o site conforme as diretrizes de acessibilidade. Durante esse trabalho, ele poderá chamar skills, como uma skill para o fluxo de pull requests ou outra para executar e gerenciar testes.

> [!NOTE]
> Não existe uma única forma correta de criar um agente personalizado. Como em qualquer uso de IA, teste e itere para encontrar o que funciona em cada ambiente e cenário.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys está comprometida em tornar sua plataforma de financiamento coletivo acessível a todas as pessoas, independentemente de suas capacidades ou preferências visuais. Comentários recentes de usuários indicaram que algumas pessoas têm dificuldade para ler o tema escuro atual por causa do contraste insuficiente entre as cores do texto e do plano de fundo. Para resolver esse problema de acessibilidade, a equipe de design solicitou um modo de alto contraste que os usuários possam ativar e desativar.

Como a acessibilidade é fundamental, queremos implementar esse recurso o mais rápido possível. Para isso, usaremos um agente personalizado.

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente.
- usar o agente de acessibilidade no Copilot Chat para implementar o modo de alto contraste.

## Revise o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado para você. Revise seu conteúdo para entender como ele orientará o Copilot.

Volte ao codespace e abra um terminal. Mude para uma branch nova, criada a partir de `main`, para trabalhar na acessibilidade. Assim, o PR de filtragem do Exercício 3 permanecerá separado:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> O frontmatter com `name` e `description` é obrigatório para agentes personalizados.

3. Leia as seções seguintes, que destacam:
   - As responsabilidades principais ao gerar código para um site acessível.
   - Práticas recomendadas de acessibilidade.
   - Exemplos de código HTML, CSS e JavaScript.
   - Armadilhas e erros comuns.

## Use o agente personalizado no Copilot Chat

O VS Code exibe todos os agentes personalizados definidos em `.github/agents` na lista suspensa de agentes, na parte inferior da exibição Copilot Chat. Você pode selecionar um agente personalizado para limitar a sessão de chat às instruções e ferramentas dele.

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam por exercício — as etapas indicarão quando for relevante.

1. Na lista suspensa de agentes da exibição Chat, selecione **Agent**, se ainda não estiver selecionado.

   ![Captura de tela do seletor de agente na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione a lista suspensa de agentes na parte inferior da exibição Chat. Ela mostra o agente ativo; por padrão, **default**.
3. Na lista de agentes disponíveis, selecione **Accessibility agent**.
4. Envie o seguinte prompt ao agente de acessibilidade:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. O Copilot Chat começará a trabalhar: explorará a base de código, proporá alterações e editará os arquivos do projeto. Cada edição aparecerá no chat com o caminho do arquivo e um diff para revisão.
6. Conforme as edições forem feitas, o indicador **Files changed** será atualizado para mostrar o conjunto de arquivos alterados pelo agente.

> [!NOTE]
> Esse processo provavelmente levará alguns minutos. O Copilot fará alterações reais no repositório: editará arquivos existentes, como componentes Astro, CSS e testes relacionados.

Você acompanhará e orientará o trabalho em andamento no próximo exercício.

## Resumo e próximos passos

Nesta lição, você explorou os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes de IA especializados em tarefas e domínios específicos. Com agentes personalizados, você pode transformar a experiência e os padrões da equipe em agentes reutilizáveis para orientar o Copilot na execução de determinados tipos de trabalho.

Você explorou:

- como agentes personalizados são definidos.
- como usar um agente personalizado no modo de agente do Copilot Chat.

Em seguida, você [acompanhará e orientará o trabalho do agente][next-lesson], revisando as alterações à medida que são feitas e adicionando um modo claro à mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Teste sua funcionalidade com o servidor MCP do Playwright][previous-lesson] | [Próxima lição: Monitoramento e gerenciamento de agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
