---
title: "Lição 8 - Crie e faça o merge do PR da funcionalidade"
description: "Revise em conjunto a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; depois, autorize o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

A implementação da filtragem, as atualizações de instruções, a atualização da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. Agora é hora de revisar tudo em conjunto e abrir um pull request. Você fez o merge do pull request (PR) de avaliação por estrelas manualmente; desta vez, permitirá que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Em geral, separaríamos a funcionalidade, as atualizações de instruções, a atualização da skill e o agente de QA em alguns PRs. Para simplificar o workshop, mantivemos todo o fluxo de filtragem e qualidade em uma sessão e branch, e incluiremos todo esse trabalho neste PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de merge.
- inspecionar o PR completo da funcionalidade e as evidências de verificação.
- autorizar o Agent Merge somente depois da revisão e confirmar que o PR foi mesclado.

## Cenário

Ao longo do fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar uma funcionalidade. Agora a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização do merge sob o controle de quem desenvolve.

## Conheça o Agent Merge

O **Agent Merge** automatiza o trabalho restante para incorporar um pull request no aplicativo GitHub Copilot. Depois de habilitá-lo, a sessão do aplicativo lê seu pull request e resolve o que estiver bloqueando o PR — corrige verificações de integração contínua (CI) com falha, responde aos comentários de revisão e faz rebase quando necessário — e faz o merge assim que o GitHub permitir. O processo é executado em segundo plano, continua após a reinicialização do aplicativo e é desativado quando o pull request é mesclado.

Até aqui, você selecionou **Merge pull request** manualmente. O Agent Merge pode assumir essa responsabilidade, mas ainda precisa de autorização explícita para editar o código e fazer o merge. Revise as ações permitidas e o trabalho antes de conceder a permissão de merge.

## Use o Agent Merge para gerenciar o PR

Com todo o código criado e revisado, vamos permitir que o Agent Merge gerencie o PR.

1. No seletor de agentes, selecione **Default agent**.
2. Selecione a lista suspensa ao lado de **Create PR**.
3. Selecione **Agent Merge**. O botão mudará para **Agent Merge**.
4. Selecione o botão **Agent Merge** para iniciar o processo. Selecionar a opção no menu, por si só, não inicia o processo.

O processo do Agent Merge começará. Ele:

- criará o pull request com título e descrição.
- se a sessão tiver sido iniciada a partir de uma issue, fará referência a ela no corpo da descrição.
- fará rebase ou resolverá possíveis conflitos com a branch de destino.
- acompanhará o processo de CI para garantir que todas as verificações sejam aprovadas.
- acompanhará comentários de outras pessoas desenvolvedoras ou da revisão de código do Copilot e fará alterações para resolver os apontamentos.
- poderá fazer merge automaticamente quando tudo estiver aprovado.

Vamos permitir que o Agent Merge também faça o merge depois que tudo for aprovado.

5. Revise o diff do PR e as evidências de verificação antes de conceder a permissão de merge.
6. Selecione a lista suspensa ao lado de **Agent Merge**.
7. Confirme se **Merge pull request** está marcado.
8. Peça explicitamente ao agente que faça o merge:

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. Acompanhe o progresso e resolva os bloqueios relatados. Confirme se o status do PR é **Merged** antes de continuar; habilitar a permissão de merge ou ver um agente em execução não confirma que o merge foi concluído.

> [!IMPORTANT]
> O Agent Merge não ignora as proteções do repositório nem permissões ausentes. Resolva esses bloqueios antes de continuar.

## Resumo e próximos passos

Você automatizou várias etapas do processo de desenvolvimento: geração de código, teste e validação e, agora, o processo do pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de merge.
- inspecionou o PR completo da funcionalidade e as evidências de verificação.
- autorizou o Agent Merge somente depois da revisão e confirmou que o PR foi mesclado.

Em seguida, você [usará um canvas existente e criará um canvas de triagem][next-lesson] para explorar uma forma mais rica de inspecionar, planejar e visualizar o trabalho com o agente.

## Recursos

- [Gerenciar issues e pull requests com o aplicativo GitHub Copilot][managing-issues-prs]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
