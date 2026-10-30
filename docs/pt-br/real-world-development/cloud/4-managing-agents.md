---
title: "Exercício 4 - Monitoramento e gerenciamento de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente de nuvem do Copilot que realizasse três tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente de nuvem trabalhe de forma autônoma e assíncrona, ainda é importante acompanhar essas tarefas.

Há várias ferramentas para gerenciar as tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nessa central de controle, você vê todas as tarefas dos agentes com pull requests abertos. Também pode explorar as operações realizadas e orientar uma sessão em andamento.

Nesta lição, você vai:

- explorar a página de agentes para acompanhar as tarefas do agente de nuvem.
- orientar uma sessão em andamento e solicitar uma funcionalidade adicional.

## Cenário

Depois de atribuir ao agente a criação de um modo de alto contraste, a equipe percebeu que seria um bom momento para adicionar também um modo claro. Como o trabalho já envolvia atualizar o estilo do site e adicionar uma opção de alternância, pareceu natural incluir esse recurso. Você vai orientar o agente para garantir que ele implemente tanto o modo claro quanto o alto contraste.

## Revise as tarefas do agente de nuvem do Copilot

Vamos verificar o status atual de todas as tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas na área principal e no painel à esquerda. Você verá as tarefas atribuídas ao Copilot, incluindo:
   - Atualização da documentação da base de código.
   - Geração de APIs para modificar jogos.
   - Adição de um modo de alto contraste ao site.
3. Selecione uma tarefa em andamento. Revise as operações já realizadas pelo Copilot. Elas podem incluir:
   - Obter o código do repositório.
   - Criar o ambiente de trabalho do Copilot.
   - Configurar servidores MCP.
   - Executar as várias etapas para concluir a tarefa atribuída.

> [!NOTE]
> As etapas listadas variam conforme o estado do trabalho do Copilot e a abordagem que ele adotou.

4. Observe também o painel de pull requests, exibido à direita. Ele permite acompanhar o pull request e os arquivos alterados.

## Oriente o agente de nuvem

Agora que você conhece as tarefas ativas, peça ao Copilot que inclua uma opção de modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão relacionada à adição do modo de alto contraste. O título exato varia conforme o nome usado pelo Copilot e o estado atual do trabalho.

    ![Sessão de acessibilidade na central de controle](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que ela indique que a configuração foi concluída e o trabalho começou. Você verá mensagens semelhantes às abaixo.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione o seguinte prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de tela da tarefa do agente de nuvem na página de agentes, com a caixa de diálogo para orientar a sessão ativa do Copilot destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot confirma o recebimento do prompt e o inclui no fluxo de trabalho.

## Deixe o Copilot trabalhar

Como antes, o Copilot trabalhará na tarefa atualizada. Ele incorporará a nova solicitação ao fluxo depois de concluir a etapa em andamento quando você enviou a mensagem.

Isso levará alguns minutos, então aproveite para fazer uma pausa e refletir sobre o que você aprendeu até aqui.

## Resumo e próximos passos

Nesta lição, você explorou a página de agentes do Copilot, a central para monitorar e orientar tarefas do agente de nuvem do GitHub Copilot. Nela, você acompanha tarefas ativas e concluídas, revisa o trabalho realizado e redireciona tarefas em andamento para ajustar o escopo ou fornecer orientações adicionais.

Você explorou:

- como usar a central de controle e a página de agentes para acompanhar tarefas do agente de nuvem.
- como redirecionar uma sessão em andamento para solicitar funcionalidades adicionais.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, vamos [iterar sobre os pull requests criados pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre os agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iteração sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
