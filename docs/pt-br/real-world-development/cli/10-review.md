---
title: "Lição 10 - Encerramento e próximos passos"
description: "Recapitule o fluxo de trabalho do Copilot CLI, os dois pull requests, as personalizações reutilizáveis e outros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Você usou o GitHub Copilot CLI ao longo de um fluxo de trabalho contínuo na Tailspin Toys. Você:

- preparou um Codespace, instalou o Copilot CLI, explorou o projeto e encontrou a issue de filtragem incluída no backlog.
- adicionou avaliações por estrelas, revisou o resultado em um navegador encaminhado e fez manualmente o merge do primeiro pull request (PR).
- começou pela issue de filtragem, definiu a abordagem no modo Plan, implementou-a no modo Autopilot e revisou o resultado no modo Interactive.
- orientou o agente com instruções personalizadas, personalizou a skill `quality-checks` existente e usou-a para executar testes de unidade, lint e verificações de tipos.
- adicionou o servidor Model Context Protocol (MCP) do Playwright e o usou para explorar os filtros em um navegador real.
- criou e selecionou um agente personalizado de garantia de qualidade (QA) para avaliar requisitos, cobertura, resultados da skill e evidências do navegador.
- revisou toda a alteração de filtragem e autorizou o Agent Merge para o PR da funcionalidade.
- explorou comandos slash para gerenciar o contexto e os modelos, compartilhar sessões e delegar tarefas à nuvem, se desejado.

## O que você entregou

O workshop tem dois marcos de PR, cada um em sua própria branch criada a partir da `main` atualizada:

1. **Avaliações por estrelas:** exiba o `starRating` existente e um estado explícito para jogos sem avaliação nos cards.
2. **Filtragem e fluxo de qualidade:** implemente a filtragem, atualize as instruções e aplique-as à funcionalidade, personalize o relatório de `quality-checks`, crie um perfil de QA e inclua os testes relacionados.

Da etapa de planejamento da filtragem até a abertura do PR, você usou a mesma conversa e a mesma branch. Reunimos esse trabalho em um único PR para simplificar o workshop.

## Diferentes tipos de verificação

Você verificou o código de várias maneiras: testes automatizados, inspeção manual no navegador e exploração do navegador pelo Copilot com MCP. A skill `quality-checks` executou testes de unidade, lint e verificações de tipos e apresentou os resultados no novo formato. O QA reuniu essas informações com a revisão dos requisitos e da cobertura de testes antes da abertura do PR.

Os testes adicionados devem preencher lacunas reais; também é válido que uma execução de QA não encontre testes adicionais necessários. Revise o código e as evidências antes de autorizar o merge e atualize as evidências afetadas depois de qualquer alteração.

## Práticas recomendadas

O contexto e as ferramentas fornecidos ao Copilot moldam o trabalho. Neste workshop, você atualizou instruções, personalizou uma skill, criou um perfil de QA e configurou um servidor MCP. Reutilize essas personalizações em outras conversas e ajuste-as conforme as necessidades da equipe mudarem. As instruções definem padrões, as skills descrevem tarefas repetíveis, os agentes personalizados definem funções especializadas e os servidores MCP conectam ferramentas externas. Revise as alterações e os resultados das ferramentas, não apenas o resumo do agente.

Escolha o **modo e o modelo** de acordo com a tarefa. Use o **Plan** para pensar em uma abordagem antes de começar, o **Interactive** para acompanhar alterações focadas e o **Autopilot** para tarefas com escopo definido. Escolha um modelo mais rápido para edições rotineiras e outro mais capaz para trabalhos complexos.

O contexto continua tão importante quanto a infraestrutura. Descrever com clareza *o que* você quer desenvolver, *por que* e *como* faz uma diferença significativa no resultado.

## Explore outras possibilidades

Você já conhece o fluxo principal. Veja outros recursos do CLI:

- `/review` para pedir ao agente de revisão de código que analise as alterações.
- `/rubber-duck` para discutir um problema e obter outra perspectiva.
- `/fleet` para coordenar subtarefas independentes em paralelo.
- `/worktree` para isolar uma tarefa.
- `/delegate` para enviar uma tarefa ao agente de nuvem do Copilot.

## Próximos passos

A melhor maneira de se familiarizar com uma ferramenta é continuar usando-a. Use-a em código de produção, em projetos pessoais ou naquele pequeno aplicativo que você imagina há anos, mas ainda não teve tempo de desenvolver. Compartilhe seus aprendizados com a equipe e aprenda com os demais. E, como sempre, explore a documentação.

Se quiser ampliar a Tailspin Toys pelo terminal, continue com a série opcional [Foundry Backer Concierge][foundry]. Para comparar outros ambientes, explore o [workshop do VS Code][vscode], o [workshop do aplicativo GitHub Copilot][app] ou o [workshop do agente de nuvem do Copilot][cloud].

## Recursos

- [Sobre o GitHub Copilot CLI][about-cli]
- [Referência de comandos do Copilot CLI][cli-reference]
- [Personalizar o Copilot CLI][customize-cli]
- [Gerenciar pull requests com o Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
