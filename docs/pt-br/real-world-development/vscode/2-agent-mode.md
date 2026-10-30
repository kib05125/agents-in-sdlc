---
title: "Exercício 2 - Adicione funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até mesmo uma atualização simples de um aplicativo costuma exigir alterações em vários arquivos e operações como executar testes. Em geral, a pessoa desenvolvedora precisa localizar todos os arquivos necessários, fazer as alterações, executar os testes, depurar, descobrir qual arquivo ficou faltando, fazer outra alteração... e assim por diante.

É aí que entra o modo de agente do Copilot.

O modo de agente do Copilot foi criado para atuar com mais autonomia no IDE. Ele trabalha de forma semelhante a uma pessoa desenvolvedora: começa explorando a estrutura do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos explorar como usar o modo de agente para adicionar funcionalidades ao site.

> [!NOTE]
> Embora os nomes sejam parecidos, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente executa tarefas no IDE, o que permite ciclos de feedback rápidos e interação direta. O agente de nuvem trabalha de forma assíncrona, como uma pessoa da equipe, e interage com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo de agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- o modo de agente do GitHub Copilot pode implementar funcionalidades na interface e na camada de dados.
- revisar alterações e testes gerados pelo modo de agente antes de incorporá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas usuárias filtrem por categoria e editora. Você já criou uma função auxiliar de editoras no exercício anterior. Agora, concluirá o trabalho restante na camada de dados, na interface e nos testes com o modo de agente do Copilot.

## Execute o site da Tailspin Toys

Antes de fazer alterações, vamos explorar o site da Tailspin Toys para entender o que ele oferece atualmente.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com temática de desenvolvimento. Ele permite listar jogos e exibir seus detalhes. O site é um único aplicativo Astro que renderiza páginas como HTML estático durante a compilação. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, por meio do Drizzle ORM — não há uma API de backend separada nem uma estrutura de interface no cliente. As funções auxiliares reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é adicionada com um pequeno `<script>` do Astro, com escopo limitado, que usa APIs padrão do DOM.

### Inicie o site

O script `npm run dev` inicia o único servidor de desenvolvimento Astro. Para executá-lo no GitHub Codespace:

1. Volte ao codespace. Você deve estar na branch `main` depois do exercício anterior.
2. Abra um novo terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie uma branch para o trabalho de filtragem e mude para ela:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute o seguinte comando para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, você verá uma mensagem com a URL, semelhante a:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo solicitar a abertura de um navegador para `http://localhost:4321`, feche-a selecionando **x**.

5. Para abrir o site, mantenha pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecione o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Em um codespace, selecionar o link `localhost` no terminal redireciona automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o codespace, que agora está hospedando o servidor Web.

### Explore o site

Com o site em execução, explore suas funcionalidades principais:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você verá uma página com mais informações, incluindo título, descrição, editora e categoria.

## Explore o backlog com o Copilot

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam por exercício — as etapas indicarão quando for relevante.

A implementação inicial do site funciona, mas queremos adicionar novos recursos. Vamos começar revisando o backlog. Ao criar seu projeto a partir do modelo, um backlog de issues foi criado automaticamente. Peça ao GitHub Copilot que mostre essas issues.

1. Na lista suspensa de agentes da exibição Chat, selecione **Agent**. O agente planeja e implementa alterações em vários arquivos de forma autônoma, executa comandos do terminal e invoca ferramentas.

   ![Captura de tela do seletor de agente na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

2. Na lista de modelos disponíveis, selecione **Claude Sonnet 4.5**.

> [!CAUTION]
> Os autores deste workshop não estão recomendando um modelo em particular. Durante a criação do workshop, usamos Claude Sonnet 4.5 e o incluímos nas instruções para que as sugestões fossem relativamente consistentes e a experiência, satisfatória. No entanto, como os LLMs são probabilísticos, é normal que as sugestões sejam diferentes das apresentadas no workshop.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot pode usar um comando MCP diferente, mas ainda deverá conseguir concluir a tarefa.

3. Peça ao Copilot que consulte o backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para executar o comando que lista todas as issues.
5. Revise a lista gerada.

Observe que o Copilot também priorizou os itens conforme a utilidade que acredita que terão para as pessoas usuárias.

## Revise os arquivos de instruções

Antes de pedir ao agente que gere o código, revise o arquivo de instruções que fornecerá contexto ao Copilot. Você usará o arquivo de instruções da [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como adicionar funcionalidades ao site.

1. No codespace, acesse `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, incluindo:
   - Uma visão geral da arquitetura.
   - Princípios de design de componentes, testabilidade e acessibilidade.
   - Links para instruções específicas para diversos tipos de arquivo, incluindo:
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> Os arquivos de instruções podem fazer referência a outros arquivos de instruções e a arquivos do projeto. Os caminhos são relativos à localização do arquivo de instruções. Assim, é possível reutilizar orientações, dividir conjuntos complexos em partes menores e fornecer exemplos e modelos.

## Implemente a filtragem

Para concluir a filtragem, serão necessárias pelo menos três alterações separadas:

- Adicionar ou aprimorar a lógica restante de filtragem na camada de dados (`src/lib/`).
- Adicionar ou atualizar testes do comportamento de filtragem.
- Atualizar a página de listagem de jogos para incluir a interface de filtragem.

Além disso, os testes precisam ser executados e aprovados antes do merge. O modo de agente do Copilot pode realizar essas tarefas. Vamos adicionar a funcionalidade.

1. Continue a conversa atual com o Copilot ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Captura de tela mostrando como selecionar o arquivo de instruções da interface](../../../_images/ex3-select-instructions-file.png)

3. Na lista suspensa da exibição Chat, confirme que **Agent** continua selecionado.

   ![Captura de tela do seletor de agente na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirme se **Claude Sonnet 4.5** ainda está selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade com base na issue relacionada do backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe o Copilot explorar o projeto e localizar os arquivos relacionados à funcionalidade, incluindo as funções auxiliares da camada de dados, a interface e os testes. Em seguida, ele alterará os arquivos e executará os testes.

   ![Captura de tela do Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, alterar arquivos e rodar testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante o processo, talvez apareçam os botões **Keep** e **Undo** no editor. Quando o Copilot terminar, você poderá aceitar ou desfazer cada alteração; não é necessário fazer isso enquanto o trabalho está em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Captura de tela de uma caixa de diálogo do Copilot Chat que pede confirmação para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Pode haver pausas e até falhas em alguns testes durante o processo. Isso é normal: o Copilot alterna entre a geração de código e os testes até concluir a tarefa sem detectar erros.

   ![Captura de tela de uma sessão completa do modo de agente do Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado e procure possíveis problemas.

> [!CAUTION]
> É sempre importante revisar o código gerado pelo Copilot ou por qualquer outra ferramenta de IA.

10. Volte ao navegador em que o site está em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Parabéns! Neste exercício, vimos como usar o modo de agente do GitHub Copilot para adicionar recursos ao site da Tailspin Toys. Você aprendeu como:

- o modo de agente do GitHub Copilot pode implementar funcionalidades na interface e na camada de dados.
- o modo de agente pode explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- revisar alterações e testes gerados pelo modo de agente antes de incorporá-los à base de código.

Agora vamos [testar o recurso com o servidor MCP do Playwright][next-lesson] e abrir um pull request.

### Exercício bônus – Implemente paginação

À medida que a lista de jogos cresce, será necessário implementar paginação. Use o que aprendeu neste exercício e peça ao Copilot que adicione esse recurso. Considere:

- seguir as práticas recomendadas existentes, incluindo o uso dos arquivos de instruções.
- decidir como a paginação funcionará: por exemplo, se as pessoas usuárias poderão selecionar o tamanho da página ou se ele será fixo.
- incluir no prompt as orientações necessárias para que o Copilot implemente o comportamento desejado.
- iterar com o GitHub Copilot, solicitando alterações e fornecendo contexto. Esse é o fluxo normal ao trabalhar com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
