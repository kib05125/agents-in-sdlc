---
title: "Lição 0 - Pré-requisitos"
description: "Prepare-se para as lições do aplicativo GitHub Copilot: instale o Node.js para o projeto Tailspin Toys e crie sua cópia do repositório a partir do modelo."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

O aplicativo GitHub Copilot é um aplicativo para desktop que serve como central para o Copilot e o GitHub. Ele oferece acesso rápido a issues e pull requests e permite desenvolver com o GitHub Copilot. Durante este workshop, você trabalhará localmente no aplicativo Tailspin Toys, criado com Astro, usando o aplicativo GitHub Copilot. Antes de começar, confirme se o Node.js está instalado no computador e, depois, instale o aplicativo Copilot.

Nesta lição, você vai:

- instalar o Node.js para executar os testes do projeto no computador.
- criar sua própria cópia do projeto Tailspin Toys a partir do modelo.

## Instale o Node.js

Várias lições pedem a um agente que desenvolva funcionalidades e execute localmente os testes da Tailspin Toys, o que requer o **[Node.js][nodejs]** — o único runtime necessário para o projeto. Instale a versão **LTS** atual.

A opção mais simples em todas as plataformas é usar o instalador oficial:

1. Abra um terminal no sistema operacional, como o Windows Terminal ou o Terminal do macOS.
2. Execute estes comandos para verificar a versão instalada do Node.js:

    ```shell
    node --version
    npm --version
    ```

3. Se os dois comandos forem executados e o Node.js atender aos requisitos descritos no README e em `package.json` do projeto, avance para a próxima seção.

> [!TIP]
> Só é necessário concluir estas etapas se o Node.js ainda não estiver instalado ou se você precisar atualizá-lo.

4. Acesse a [página de download do Node.js][node-download].
5. Baixe a versão **LTS** para seu sistema operacional.
6. Execute o instalador e aceite as opções padrão. No Windows, mantenha selecionada a opção **Add to PATH**.
7. Depois da instalação, abra uma nova janela de terminal.
8. Confirme a instalação executando:

    ```bash
    node --version
    npm --version
    ```

9. Você verá as versões instaladas do Node.js e do npm.

> [!IMPORTANT]
> O agente do aplicativo Copilot também precisa executar `node --version` e `npm --version`. O fato de os comandos funcionarem no terminal habitual não garante que estejam disponíveis para o aplicativo. Feche o aplicativo completamente e abra-o novamente depois de instalar o Node.js ou alterar o `PATH`. Se algum dos comandos ainda falhar em uma sessão, siga as [etapas para solucionar problemas do ambiente da sessão][session-environment] antes de continuar.

> [!IMPORTANT]
> Cada worktree também precisa das dependências do projeto e do Chromium do Playwright para executar verificações E2E. Siga o README do repositório Tailspin Toys para preparar um worktree e revise as solicitações de instalação antes de aprová-las.

## Configure o repositório do laboratório

Você trabalhará na sua própria cópia do projeto Tailspin Toys. Crie-a agora a partir do [repositório de modelo][template-repository]. O novo repositório contém todos os arquivos necessários para o laboratório; você o conectará ao instalar o aplicativo.

1. Em uma nova janela do navegador, acesse o repositório do laboratório no GitHub: `https://github.com/github-samples/tailspin-toys`.
2. Crie sua cópia selecionando **Use this template** na página do repositório do laboratório. Em seguida, selecione **Create a new repository**.

    ![Botão Use this template com a opção Create a new repository selecionada no menu suspenso](../../../_images/app-0-use-template.png)

3. Se você estiver participando de um evento conduzido pelo GitHub ou pela Microsoft, siga as instruções fornecidas pelas pessoas mentoras. Caso contrário, crie o novo repositório em uma organização na qual você tenha acesso ao GitHub Copilot.

    ![Formulário Create a new repository com github-samples/tailspin-toys definido como modelo e o nome do repositório preenchido](../../../_images/app-0-create-repository.png)

4. Anote o caminho do repositório criado (**organization-or-user-name/repository-name**), pois ele será usado mais adiante no laboratório.

> [!NOTE]
> Ao criar o repositório a partir do modelo, um backlog de issues do GitHub será criado automaticamente. Você usará essas issues durante o workshop — não é necessário criar nenhuma.

Use uma cópia nova do modelo do workshop. Ela inclui instruções do repositório, o código do aplicativo, testes, uma skill `quality-checks` e uma extensão de canvas existente. Durante o workshop, você personalizará a skill e criará um agente de QA. Se estiver usando uma cópia antiga, confirme com a pessoa que está facilitando se ela contém os arquivos necessários.

## Resumo e próximos passos

Tudo pronto! Nesta lição, você:

- instalou o Node.js para que o projeto possa ser compilado e testado no computador.
- criou sua cópia do repositório Tailspin Toys a partir do modelo.

Em seguida, você [instalará o aplicativo GitHub Copilot][next-lesson], conectará o repositório criado e conhecerá o workspace.

## Recursos

- [Baixar o Node.js][node-download]
- [Criar um repositório a partir de um modelo][template-repository]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#inicie-uma-sessao-a-partir-da-issue
