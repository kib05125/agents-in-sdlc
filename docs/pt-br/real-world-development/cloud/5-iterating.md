---
title: "Exercício 5: Iteração sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciamento de agentes][previous-lesson] |
|:--|

## Revise o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que documentasse o código, criasse um recurso de jogos relacionados para a equipe de design iterar e implementasse recursos de acessibilidade, incluindo opções de alto contraste e modo claro. Vamos revisar as alterações e, se necessário, enviar comentários ao Copilot para melhorar o resultado.

### Cenário

Como já destacamos várias vezes, os fundamentos do design de software e do DevOps não mudam com a chegada da IA generativa. Sempre precisamos revisar o código gerado e seguir nosso processo normal de DevOps. Com isso em mente, vamos revisar as sugestões do GitHub Copilot para documentação, jogos relacionados e acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem do Copilot executa tarefas de forma assíncrona e sem supervisão, há restrições de segurança para manter o ambiente protegido. Entre elas:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch que usará para o código.
- O agente de nuvem é executado no GitHub Actions, que cria um ambiente separado e temporário para o trabalho.
- Uma pessoa precisa aprovar todos os workflows do GitHub Actions antes da execução.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo o acesso a servidores MCP.

## Revise a documentação gerada

Vamos começar pelo primeiro pull request (PR) criado pelo agente de nuvem do GitHub Copilot: o que adiciona documentação ao código. Você fará essa revisão usando a interface padrão de PRs do GitHub.com.

> [!NOTE]
> Ao abrir o PR, você poderá ver um aviso de que o GitHub Copilot foi bloqueado por um firewall. Isso é **esperado**, pois o acesso do Copilot a recursos externos — incluindo chamadas a servidores MCP externos — é limitado por padrão. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem do Copilot][agent-firewall].

1. Volte ao seu repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o pull request com um título semelhante a **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request terá a marca **[WIP]**. Nesse caso, aguarde a conclusão do trabalho. Isso pode levar alguns minutos; aproveite para fazer uma pausa ou refletir sobre o que aprendeu.

4. Quando o pull request estiver pronto, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, que inclui os comentários TSDoc recém-criados e outras documentações. As alterações exatas variam.

    Ao examinar as alterações, procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentário. Essas orientações vêm dos arquivos de instruções personalizadas que você revisou no início do Exercício 2.

6. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
7. Você verá um indicador de que alguns workflows estão aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovação e execução de workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows entrarão na fila na seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.

## Solicite alterações ao GitHub Copilot

Trabalhar com o Copilot em um pull request não é uma via de mão única. Você pode mencionar o Copilot em comentários do pull request, como faria com outras pessoas da equipe, ou em comentários de linha no código. O Copilot verá esses comentários e iniciará outra sessão para atender à solicitação. Como os resultados não são determinísticos, não há um texto prescritivo para usar. Algumas ideias de solicitações:

- Adicionar cabeçalhos de comentário no início de cada arquivo de código com uma breve descrição de sua finalidade.
- Adicionar comentários TSDoc a arquivos TypeScript e Astro.
- Criar um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração à documentação gerada e mencione **@copilot**, como faria com qualquer pessoa. Use uma das ideias acima ou solicite outra melhoria na documentação do código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe como ele inicia uma nova sessão para fazer as atualizações.
3. Selecione **Back to pull request** para voltar ao pull request.

    ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot terminar as alterações, um novo commit aparecerá no pull request.
5. Selecione a guia **Files changed** para revisar as alterações.

Continue iterando até ficar satisfeito. Depois, você pode marcar o PR como pronto para revisão e fazer o merge na branch principal.

![Marcar o PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revise o recurso de jogos relacionados

Vamos voltar ao PR que o Copilot criou para resolver a issue de exibir jogos relacionados na página de detalhes de um jogo.

1. Volte ao seu repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Show related games on the game details page**.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
6. Você verá um indicador de que alguns workflows estão aguardando aprovação.
7. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovação e execução de workflows](../../../_images/shared-approve-workflows.png)

8. Os workflows entrarão na fila na seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** você pode mudar para essa branch no Codespace para testar manualmente o recurso de jogos relacionados. Acesse o Codespace, abra o terminal e execute os comandos a seguir (substitua `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O Copilot criou o recurso de jogos relacionados. Como antes, você pode iterar com o agente de nuvem para solicitar atualizações. Por exemplo, peça ao Copilot para ajustar a quantidade de jogos exibidos ou adicionar cabeçalhos de comentário e comentários TSDoc (lembre-se: a issue foi atribuída **antes** de você atualizar as instruções personalizadas). Para isso, adicione um comentário na guia **Conversation**; o Copilot o verá e iniciará uma nova sessão.

## Revise os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados pelo agente personalizado. Esse PR deve incluir o modo de alto contraste solicitado no Exercício 3 e o modo claro pedido no painel de agentes do Exercício 4.

1. Volte ao seu repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request terá a marca **[WIP]**. Nesse caso, aguarde a conclusão do trabalho. Isso pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação, prestando atenção especial a:
   - Os componentes de alternância entre modos.
   - O uso do armazenamento local para manter as preferências do usuário.
   - As alterações de CSS ou estilo para os modos de alto contraste e claro.
   - Os atributos de acessibilidade (rótulos ARIA, navegação pelo teclado etc.).
   - O código JavaScript/TypeScript que gerencia a alternância entre modos.

6. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
7. Você verá um indicador de que alguns workflows estão aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovação e execução de workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows entrarão na fila na seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** você pode mudar para essa branch no Codespace e testar manualmente os recursos de acessibilidade. Acesse o Codespace, abra o terminal e execute os comandos a seguir (substitua `<branch-name>` pelo nome da branch criada pelo Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste os modos de alto contraste e claro no navegador. Confirme se funcionam e se as preferências continuam valendo após recarregar a página.

Observe como o agente personalizado de acessibilidade orientou o Copilot a implementar esses recursos seguindo práticas recomendadas de acessibilidade. Se encontrar problemas ou oportunidades de melhoria, mencione **@copilot** em um comentário para solicitar atualizações, como você fez nos PRs anteriores.

## Exercício opcional — continue delegando

O agente de nuvem é mais útil quando você pode delegar itens reais do backlog e se concentrar em outras tarefas. Para desenvolver esse hábito, crie mais algumas issues no seu repositório e atribua-as ao Copilot. Algumas ideias:

- Criar um formulário para manifestar interesse em um jogo na página de detalhes.
- Implementar paginação na lista de jogos.
- Adicionar validação de entrada e tratamento de erros às funções auxiliares de acesso a dados.

## Resumo

Você concluiu o roteiro do agente de nuvem. Nestas lições, você:

- **Examinou os arquivos de instruções personalizadas incluídos no repositório** para ver como influenciariam mais tarde o resultado do agente de nuvem.
- **Atribuiu issues ao agente de nuvem do Copilot** e acompanhou a preparação do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** para acessibilidade e adicionou opções de alto contraste e modo claro.
- **Usou a página de agentes do Copilot como central de controle** para acompanhar e orientar a sessão de acessibilidade em andamento.
- **Revisou e ajustou pull requests do agente de nuvem**, mencionando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximos passos

Você concluiu o roteiro do agente de nuvem. Se quiser continuar explorando, os outros roteiros complementam o que você praticou:

- 🖥️ **[Roteiro do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP diretamente no IDE.
- 💻 **[Roteiro do CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, agent skills, um agente de QA personalizado, Playwright MCP, Agent Merge e uma integração opcional `/delegate` com o agente de nuvem.

No seu próprio repositório, experimente estas ideias:

- Atribua ao agente de nuvem uma issue de refatoração ou cobertura de testes.
- Crie um agente personalizado para outro domínio.
- Configure `copilot-setup-steps.yml` para outra pilha de tecnologias.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuição de issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]
- [Configuração do firewall do agente de nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Gerenciamento de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
