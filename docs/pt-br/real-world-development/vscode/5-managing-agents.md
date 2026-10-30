---
title: "Exercício 5 - Monitoramento e gerenciamento de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

No modo de agente, o GitHub Copilot trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho acontece em tempo real no seu computador, o Copilot Chat no VS Code mostra cada chamada de ferramenta e edição de arquivo conforme ocorre. Você pode revisar cada diff no chat, aceitar ou rejeitar alterações individuais e orientar a conversa com prompts de acompanhamento sem sair do editor.

Neste exercício, você vai:

- acompanhar em tempo real o trabalho do agente de acessibilidade.
- revisar e aceitar as edições propostas nos arquivos.
- orientar o agente durante a sessão para adicionar um modo claro ao lado do modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho acontece, a equipe de design faz outra solicitação: também quer uma opção de modo claro para as pessoas que consideram o tema padrão escuro demais. Em vez de começar uma conversa do zero, você orientará a sessão atual para expandir o trabalho.

## Acompanhe o progresso do Copilot

A exibição do Copilot Chat mostra cada etapa do agente: os arquivos explorados, os comandos executados e as edições propostas. Cada edição aparece no chat com um diff e os controles **Keep** e **Undo**.

1. Volte à exibição Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, percorra o chat para acompanhar as chamadas de ferramentas. Você verá leituras e edições de arquivos e os comandos de terminal cuja execução ele solicitar.
3. Quando o agente propuser uma edição, o diff aparecerá no chat:
   - Selecione **Keep** para aceitar a alteração.
   - Selecione **Undo** para desfazê-la e permitir que o agente tente novamente.
4. Você também pode abrir o indicador **Files changed** na parte superior do chat para ver todos os arquivos alterados pelo agente durante a sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão — por exemplo, para executar um comando do terminal —, o agente pausará e exibirá uma solicitação **Allow**. Aprove apenas os comandos que considerar seguros.

5. Aguarde o agente terminar de implementar o modo de alto contraste. Ele exibirá uma mensagem final resumindo as alterações.

## Oriente o agente para adicionar o modo claro

Agora que o modo de alto contraste está pronto, você ampliará a mesma conversa para adicionar uma opção de modo claro. Como continuará na mesma sessão de chat, o agente manterá todo o contexto do trabalho anterior, incluindo os componentes alterados, a abordagem de estilo e o padrão de armazenamento local.

1. No campo de entrada do Copilot Chat, envie este prompt de acompanhamento:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. O agente continuará o trabalho, explorará as alterações já feitas e proporá adições para o modo claro.
3. Revise cada nova edição quando aparecer, como fez com o modo de alto contraste. Use **Keep** ou **Undo** em cada diff.
4. Se o agente seguir uma direção indesejada — por exemplo, tentar refatorar a interface de alternância de uma forma que você não quer —, interrompa-o com o botão **Stop** ao lado do campo de entrada e envie uma mensagem para esclarecer.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Use prompts de acompanhamento para solicitar ajustes (“simplifique o CSS — use variáveis CSS em vez de duplicar seletores”), corrigir o rumo (“não adicione uma terceira opção; integre o modo claro ao painel de configurações existente”) ou ampliar o escopo (“adicione também atalhos de teclado para as duas opções”).

## Revise o conjunto de alterações

Antes de fazer commit, revise rapidamente tudo o que o agente alterou.

1. Abra a exibição **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá ver atualizações nos componentes Astro, estilos e testes relacionados.
3. Abra alguns arquivos alterados e examine os diffs. Confirme se as práticas de acessibilidade recomendadas pelo agente personalizado foram aplicadas: atributos ARIA, navegação pelo teclado, HTML semântico e persistência com armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações pelo painel Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximos passos

Você usou o modo de agente do Copilot Chat para acompanhar e orientar uma sessão em andamento: revisou cada edição, aceitou ou rejeitou alterações e enviou prompts de acompanhamento para expandir o trabalho sem perder o contexto.

Nesta lição, você explorou:

- como acompanhar o trabalho de um agente em tempo real.
- como aceitar ou rejeitar edições individuais.
- como orientar uma sessão em andamento com prompts de acompanhamento.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade em um pull request e revisará as alterações produzidas pelo agente personalizado local durante este laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iteração sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
