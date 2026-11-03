---
title: "Lição 6 - Valide a funcionalidade com Playwright MCP"
description: "Inspecione ou configure o Playwright MCP no Copilot CLI e use-o para explorar o recurso de filtragem no navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como já destacamos, escrever código envolve mais do que apenas programar. É preciso trabalhar com dados, serviços externos e até permitir que o Copilot use outras automações. É aí que entram os servidores MCP. Eles permitem que o Copilot vá além dos recursos integrados ao CLI, oferecendo mais ferramentas e serviços.

Nesta lição, você vai:

- entender o que é o Model Context Protocol (MCP) e como o Copilot CLI o usa.
- adicionar o servidor MCP do Playwright, caso ele ainda não esteja disponível.
- pedir ao agente que controle um navegador e explore o recurso de filtragem.

## Cenário

Os testes unitários e de ponta a ponta são importantes, mas validar alterações na interface do usuário exige interagir com ela. Você quer permitir que o Copilot use o site como uma pessoa usuária faria, automatizando ainda mais o processo de alteração e aumentando a confiança de que as atualizações funcionam como esperado.

## O que é o Model Context Protocol (MCP)?

O [Model Context Protocol (MCP)][mcp-blog-post] oferece aos agentes de IA uma maneira de se comunicar com ferramentas e serviços externos. Com o MCP, os agentes de IA podem se comunicar com ferramentas e serviços externos em tempo real. Assim, podem acessar informações atualizadas e realizar ações em seu nome.

Essas ferramentas e esses recursos são acessados por meio de um servidor MCP, que atua como uma ponte entre o agente de IA e as ferramentas e os serviços externos. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente de IA pode acessar.

Alguns servidores MCP conhecidos são:

- **[GitHub MCP Server][github-mcp]**: fornece acesso a APIs para gerenciar repositórios, issues e pull requests do GitHub.
- **[Playwright MCP Server][playwright-mcp-server]**: oferece recursos de automação de navegadores com o Playwright.

Há muitos outros servidores MCP disponíveis. O GitHub hospeda um [registro de MCP][mcp-registry] para facilitar a descoberta de servidores e as contribuições ao ecossistema.

> [!CAUTION]
> Trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um, revise o código-fonte, verifique o publicador e considere as implicações de segurança.

## Adicione o servidor MCP do Playwright

Se o servidor MCP do Playwright ainda não estiver disponível, adicione-o para que o Copilot possa interagir com o site como uma pessoa usuária faria.

> [!NOTE]
> Se o Playwright MCP já estiver configurado no Copilot CLI, ignore estas etapas de configuração e continue na próxima seção.

1. Volte ao Codespace.
2. No Copilot CLI, abra a caixa de diálogo para adicionar um servidor MCP com o seguinte comando:

   ```plaintext
   /mcp add
   ```

3. Em nome, digite `playwright` e pressione <kbd>Tab</kbd>.
4. Confirme que o tipo de servidor é **STDIO** pressionando <kbd>Enter</kbd> e, em seguida, pressione <kbd>Tab</kbd>.
5. Cole o seguinte na caixa de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pressione <kbd>Control</kbd>+<kbd>S</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para salvar o novo servidor MCP.
7. Pressione <kbd>Esc</kbd> para sair da caixa de diálogo MCP.

## Peça ao Copilot que explore o recurso com o Playwright

Você já confirmou manualmente que a funcionalidade se comporta como esperado. Agora, vamos usar o servidor MCP do Playwright disponível no Copilot para fazer o mesmo!

1. Use o seguinte prompt para pedir ao Copilot que valide a funcionalidade com o servidor MCP do Playwright:

   ```plaintext
   Inicie o aplicativo e use o Playwright MCP para verificar a filtragem de acordo com a issue e nosso plano. Informe o que funciona e o que não funciona, sem fazer alterações. Quando terminar, pare o servidor iniciado.
   ```

> [!NOTE]
> Não é necessário dizer ao Copilot que use o servidor MCP, pois ele geralmente o identifica por conta própria. Mas, como você sabe qual ferramenta deve usar, indicar isso pode ajudar a direcioná-lo, garantir resultados mais consistentes e economizar alguns tokens.

2. Acompanhe as etapas que o Copilot executa no navegador para confirmar se a funcionalidade está funcionando.
3. Leia o relatório e confirme que tudo se comporta como esperado.

O Copilot iniciará o servidor, usará o Playwright para interagir com o site, parará o servidor e fornecerá um relatório.

## Resumo e próximos passos

Parabéns, você usou o servidor MCP do Playwright para explorar seu recurso em um navegador real a partir do Copilot CLI! Em resumo, você:

- aprendeu o que é o Model Context Protocol (MCP) e como o Copilot CLI o usa.
- adicionou o servidor MCP do Playwright, caso ele ainda não estivesse disponível.
- pediu ao agente que controlasse um navegador e explorasse o recurso de filtragem.

Em seguida, você [criará um agente personalizado de QA][next-lesson] que combina a skill e as ferramentas do navegador em uma função especializada.

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
