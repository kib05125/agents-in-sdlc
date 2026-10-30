---
title: "Lição 8 - Crie e faça o merge do PR da funcionalidade"
description: "Revise em conjunto a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; depois, crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação da filtragem, as atualizações de instruções, a atualização da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. Agora é hora de revisar tudo em conjunto e abrir um pull request. Você fez o merge do pull request (PR) de avaliação por estrelas manualmente; desta vez, permitirá que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Em geral, separaríamos a funcionalidade, as atualizações de instruções, a atualização da skill e o agente de QA em alguns PRs. Para simplificar o workshop, mantivemos todo o fluxo de filtragem e qualidade em uma conversa e branch e incluiremos todo esse trabalho neste PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de merge.
- inspecionar toda a alteração da funcionalidade e as evidências de verificação.
- criar o PR de filtragem.
- habilitar o Agent Merge somente depois da revisão e confirmar que o PR foi mesclado.

## Cenário

Ao longo do fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar uma funcionalidade. Agora a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização do merge sob o controle de quem desenvolve.

## Conheça o Agent Merge

O **Agent Merge** automatiza o trabalho restante para incorporar um pull request. Depois de habilitá-lo, o Copilot resolve o que estiver bloqueando o PR — corrige verificações de integração contínua (CI) com falha, responde aos comentários de revisão e faz rebase quando necessário — e, em seguida, habilita o merge automático do GitHub quando o repositório permite.

Até aqui, você selecionou **Merge pull request** manualmente. O Agent Merge pode assumir essa responsabilidade. Antes de habilitá-lo, revise o trabalho e confirme que ele está pronto.

## Use o Agent Merge para gerenciar o PR

Com todo o código criado, vamos revisar as alterações, criar o PR e deixar o Agent Merge gerenciar as próximas etapas.

1. Volte ao Codespace.
2. Abra a caixa de diálogo de agente digitando `/agent`.
3. Selecione **Default** na lista e pressione <kbd>Enter</kbd>.
4. Crie um novo PR com o comando `/pr create`.
5. Ative o Agent Merge com `/pr agentmerge`.
6. O Copilot acompanhará o processo de integração contínua do PR. Quando tudo for aprovado, ele fará o merge.
7. Confirme se o Copilot exibe uma mensagem semelhante a “PR #14 was squash-merged successfully”.

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branch, filas de merge, configurações do repositório nem permissões ausentes. Se houver um bloqueio, leia o motivo informado e, se o repositório permitir, faça manualmente o merge que você já revisou.

## Resumo e próximos passos

Você automatizou várias etapas do processo de desenvolvimento: geração de código, teste e validação e, agora, o processo do pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de merge.
- inspecionou toda a alteração da funcionalidade e as evidências de verificação.
- criou o PR de filtragem.
- habilitou o Agent Merge somente depois da revisão e confirmou que o PR foi mesclado.

Em seguida, você [explorará outros comandos slash úteis do Copilot CLI][next-lesson] para gerenciar o contexto, escolher modelos, compartilhar sessões e, opcionalmente, delegar tarefas à nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
