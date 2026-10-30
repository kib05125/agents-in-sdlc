---
title: "Lição 6 - Valide a funcionalidade com Playwright MCP"
description: "Inspecione ou configure o Playwright MCP no Copilot CLI e use-o para explorar a funcionalidade de filtragem em um navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como já destacamos, escrever código envolve mais do que apenas escrever código. É preciso trabalhar com dados, serviços externos e automações adicionais que o Copilot possa usar. É para isso que servem os servidores MCP. Eles permitem que o Copilot vá além dos recursos integrados à CLI, oferecendo mais ferramentas e serviços.

Nesta lição, você vai:

- entender o que é o Model Context Protocol (MCP) e como o Copilot CLI o usa.
- adicionar o servidor MCP do Playwright, se ele ainda não estiver disponível.
- pedir ao agente que controle um navegador e explore a funcionalidade de filtragem.

## Cenário

Testes de unidade e de ponta a ponta são importantes, mas validar alterações na interface exige interagir com ela. Você quer permitir que o Copilot use o site como uma pessoa usuária faria, automatizando ainda mais as alterações e aumentando a confiança de que elas funcionam conforme esperado.

## O que é o Model Context Protocol (MCP)?

O [Model Context Protocol (MCP)][mcp-blog-post] permite que agentes de IA se comuniquem com ferramentas e serviços externos. Com MCP, os agentes podem acessar informações atualizadas e executar ações em seu nome.

Essas ferramentas e esses recursos são acessados por meio de um servidor MCP, que atua como ponte entre o agente de IA e as ferramentas e serviços externos. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente pode acessar.

Alguns servidores MCP conhecidos:

- **[Servidor MCP do GitHub][github-mcp]**: fornece acesso a APIs para gerenciar repositórios, issues e pull requests do GitHub.
- **[Servidor MCP do Playwright][playwright-mcp-server]**: oferece recursos de automação de navegadores com o Playwright.

Há muitos outros servidores MCP disponíveis. O GitHub mantém um [registro MCP][mcp-registry] para facilitar a descoberta de servidores e a colaboração no ecossistema.

> [!CAUTION]
> Trate servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor, revise o código-fonte, verifique quem o publicou e considere as implicações de segurança.

## Adicione o servidor MCP do Playwright

Se o servidor MCP do Playwright ainda não estiver disponível, adicione-o para que o Copilot possa interagir com o site como uma pessoa usuária.

> [!NOTE]
> Se o Playwright MCP já estiver configurado no Copilot CLI, ignore estas etapas e continue na próxima seção.

1. Volte ao Codespace.
2. Abra a caixa de diálogo para adicionar um servidor MCP digitando o seguinte comando no Copilot CLI:

   ```plaintext
   /mcp add
   ```

3. Digite `playwright` como nome e pressione <kbd>Tab</kbd>.
4. Confirme o tipo de servidor **STDIO** pressionando <kbd>Enter</kbd> e, depois, pressione <kbd>Tab</kbd>.
5. Cole o comando a seguir na caixa **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pressione <kbd>Control</kbd>+<kbd>S</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para salvar o novo servidor MCP.
7. Pressione <kbd>Esc</kbd> para sair da caixa de diálogo do MCP.

## Peça ao Copilot que explore a funcionalidade com o Playwright

Você já confirmou manualmente que a funcionalidade funciona. Agora vamos usar o servidor MCP do Playwright disponível no Copilot para fazer o mesmo.

1. Use este prompt para pedir ao Copilot que valide a funcionalidade com o servidor MCP do Playwright:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Não é necessário pedir explicitamente que o Copilot use o servidor MCP, pois ele geralmente o identifica por conta própria. Mas, como você sabe qual ferramenta deve ser usada, indicar isso pode ajudar a obter resultados mais consistentes e economizar tokens.

2. Observe o Copilot listar as etapas executadas no navegador para confirmar se a funcionalidade funciona.
3. Leia o relatório e confirme que tudo está funcionando como esperado.

O Copilot iniciará o servidor, usará o Playwright para interagir com o site, encerrará o servidor e fornecerá um relatório.

## Resumo e próximos passos

Parabéns, você usou o servidor MCP do Playwright para explorar sua funcionalidade em um navegador real pelo Copilot CLI. Para recapitular, você:

- aprendeu o que é o Model Context Protocol (MCP) e como o Copilot CLI o usa.
- adicionou o servidor MCP do Playwright, se ele ainda não estava disponível.
- pediu ao agente que controlasse um navegador e explorasse a funcionalidade de filtragem.

Em seguida, você [criará um agente personalizado de QA][next-lesson] que combinará a skill e as ferramentas de navegador em uma função especializada.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Adicionar servidores MCP ao Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
