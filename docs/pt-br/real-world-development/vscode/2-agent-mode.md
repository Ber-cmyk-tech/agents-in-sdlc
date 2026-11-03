---
title: "Exercício 2 - Adicione uma funcionalidade com o Agent Mode do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até as atualizações mais simples de um aplicativo costumam exigir alterações em vários arquivos e operações como executar testes. Em geral, o fluxo de trabalho de uma pessoa desenvolvedora envolve localizar todos os arquivos necessários, fazer as alterações, executar os testes, depurar, descobrir qual arquivo ficou faltando, fazer outra alteração... e assim por diante.

É aí que entra o Agent Mode do Copilot.

O Agent Mode do Copilot foi criado para atuar de forma mais autônoma no IDE. Ele trabalha de maneira semelhante a uma pessoa desenvolvedora: começa explorando a estrutura do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos explorar como usar o Agent Mode para adicionar funcionalidades ao site.

> [!NOTE]
> Embora os nomes sejam parecidos, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente trabalha no IDE, o que permite ciclos de feedback rápidos e interação direta. O agente de nuvem foi criado para trabalhar de forma assíncrona como uma pessoa da equipe e interagir com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o Agent Mode do Copilot pode explorar o projeto, identificar os arquivos relevantes e coordenar alterações.
- o Agent Mode do GitHub Copilot pode implementar funcionalidades na UI e na camada de dados.
- revisar alterações e testes gerados pelo Agent Mode do Copilot antes de mesclá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem por categoria e editora. No exercício anterior, você adicionou um auxiliar de editoras. Agora, terminará o restante do trabalho na camada de dados, na UI e nos testes com o Agent Mode do Copilot.

## Execute o site da Tailspin Toys

Antes de fazer alterações, vamos explorar o site da Tailspin Toys para entender a funcionalidade atual.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com temática de desenvolvimento. Ele permite listar jogos e exibir detalhes sobre eles. O site é um único aplicativo Astro que renderiza as páginas como HTML estático durante o build. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, usando o ORM Drizzle — não há uma API de backend separada nem um framework de UI no cliente. Os auxiliares reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é adicionada com um pequeno elemento `<script>` do Astro com escopo limitado, usando as APIs padrão do DOM.

### Inicie o site

Para facilitar a execução do site, há uma tarefa `npm run dev`, que inicia o único servidor de desenvolvimento Astro. Siga estas etapas para executá-lo no GitHub Codespace:

1. Volte ao codespace. Você deverá estar em `main` depois do exercício anterior.
2. Abra um novo terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie e selecione um novo branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute este comando para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor de desenvolvimento Astro estiver pronto, você verá um aviso com a URL, semelhante a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo pedir para abrir uma janela do navegador para `http://localhost:4321`, feche-a selecionando **x**.

5. Abra o site mantendo <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) pressionado e selecionando o endereço do servidor Astro, `http://localhost:4321`, no terminal.

> [!NOTE]
> Em um codespace, selecionar um link de URL localhost no terminal redireciona automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o codespace, que agora hospeda o servidor Web.

### Explore o site

Com o site em execução, explore sua funcionalidade. Os principais recursos incluem:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você verá uma página de detalhes com mais informações, incluindo título, descrição, editora e categoria.

## Explore o backlog com o Copilot

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e a seleção do modelo variam em cada exercício; as etapas correspondentes explicarão quando forem relevantes.

A implementação inicial do site funciona, mas queremos aprimorá-la com novos recursos. Vamos começar revisando o backlog. Ao criar seu projeto a partir do modelo, um backlog de issues foi criado automaticamente — peça ao GitHub Copilot para exibir esses itens.

1. No menu suspenso de agentes da exibição Chat, selecione **Agent**. O agente planeja e implementa alterações em vários arquivos de forma autônoma, executa comandos no terminal e chama ferramentas.

   ![Captura de tela do seletor de agentes na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

2. Na lista de modelos disponíveis, selecione **Claude Sonnet 4.5**.

> [!CAUTION]
> As pessoas autoras deste workshop não estão recomendando um modelo específico. Ao criar o workshop, usamos o Claude Sonnet 4.5 e o incluímos nas instruções na esperança de que as sugestões de código sejam relativamente consistentes e proporcionem uma boa experiência. No entanto, como os LLMs são probabilísticos, as sugestões podem ser diferentes das indicadas no workshop. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot talvez use um comando MCP diferente, mas ainda deverá conseguir concluir a tarefa.

3. Pergunte ao Copilot sobre o backlog enviando este prompt:

   ```plaintext
   Mostre o backlog de itens do meu repositório do GitHub. Ajude-me a priorizá-los de acordo com o que será mais útil para as pessoas usuárias.
   ```

4. Selecione **Continue** para executar o comando que lista todas as issues.
5. Revise a lista de issues gerada.

Observe que o Copilot também priorizou os itens de acordo com o que considera mais útil para as pessoas usuárias.

## Revise os arquivos de instruções

Antes de pedir ao agente que gere o código, este é um bom momento para revisar o arquivo de instruções que fornecerá contexto ao Copilot. Vamos usar o arquivo de instruções de [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que contém orientações sobre como adicionar funcionalidades ao site.

1. No codespace, acesse `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura.
   - princípios para design de componentes, capacidade de teste e acessibilidade.
   - links para arquivos de instruções específicos de vários tipos de arquivo, incluindo:
     - Astro.
     - Tailwind CSS.
     - Drizzle.

> [!TIP]
> Os arquivos de instruções podem fazer referência a outros arquivos de instruções e a arquivos do projeto. Os caminhos são relativos ao local do arquivo de instruções. Assim, é possível reutilizar orientações, dividir conjuntos complexos em partes menores e mais gerenciáveis e fornecer exemplos e modelos.

## Implemente a funcionalidade de filtragem

Para concluir a filtragem, serão necessárias pelo menos três alterações separadas no aplicativo:

- adicionar ou aprimorar a lógica restante de filtragem na camada de dados (`src/lib/`).
- adicionar ou atualizar testes do comportamento de filtragem.
- atualizar a página de listagem de jogos para incluir a UI de filtragem.

Além disso, os testes precisam ser executados e aprovados antes de mesclar as alterações à base de código. O Agent Mode do Copilot pode executar essas tarefas. Vamos adicionar a funcionalidade.

1. Continue a conversa atual com o Copilot ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Captura de tela com um exemplo de seleção do arquivo de instruções de UI](../../../_images/ex3-select-instructions-file.png)

3. No menu suspenso de agentes da exibição Chat, confirme que **Agent** continua selecionado.

   ![Captura de tela do seletor de agentes na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade com base na issue relacionada do backlog usando este prompt:

   ```plaintext
   Atualize o site para incluir a filtragem por editora e categoria de acordo com os requisitos da issue relacionada no backlog do GitHub. O auxiliar de editoras já existe desde o exercício anterior; preserve e refine esse trabalho conforme necessário enquanto conclui as alterações restantes na camada de dados, na UI e nos testes. Confirme que todos os testes foram aprovados antes de terminar. O servidor já está em execução, então não é necessário iniciá-lo.
   ```

6. Observe enquanto o Copilot explora o projeto e localiza os arquivos relacionados à funcionalidade desejada. Você verá que ele encontra os auxiliares da camada de dados, a UI e os testes. Em seguida, começa a modificar os arquivos e a executar os testes.

   ![Captura de tela do Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, modificar arquivos e executar testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante o processo, talvez apareçam os botões **Keep** e **Undo** no editor. Quando o Copilot terminar, você verá um botão **Keep** ou **Undo** para cada alteração; não é necessário selecioná-los enquanto o trabalho estiver em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Captura de tela de uma caixa de diálogo no painel do Copilot Chat que pede confirmação para executar os testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Talvez ocorram pausas e alguns testes falhem durante o processo. Não há problema! O Copilot alterna entre gerar código e executar testes até concluir a tarefa e não detectar erros.

   ![Captura de tela de uma sessão completa do Agent Mode do Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado para identificar possíveis problemas.

> [!CAUTION]
> Lembre-se: é importante revisar sempre o código gerado pelo Copilot ou por qualquer outra ferramenta de IA.

10. Volte ao navegador em que o site está em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Parabéns! Neste exercício, exploramos como usar o Agent Mode do GitHub Copilot para adicionar recursos ao site da Tailspin Toys. Aprendemos como:

- o Agent Mode do GitHub Copilot pode implementar funcionalidades na UI e na camada de dados.
- o Agent Mode do Copilot pode explorar o projeto, identificar os arquivos relevantes e coordenar alterações.
- revisar alterações e testes gerados pelo Agent Mode do Copilot antes de mesclá-los à base de código.

Agora, vamos [testar o recurso com o servidor MCP do Playwright][next-lesson] e abrir um pull request.

### Exercício bônus de exploração — implemente a paginação

À medida que a lista de jogos cresce, será necessário implementar paginação. Usando o que aprendeu neste exercício, peça ao Copilot que atualize o site para incluir a paginação. Considere estes pontos:

- siga as práticas recomendadas existentes, inclusive o uso dos arquivos de instruções do projeto.
- decida como a paginação funcionará: as pessoas usuárias poderão selecionar o tamanho da página ou ele será fixo?
- ao elaborar o prompt, forneça ao Copilot as orientações necessárias para implementar a solução desejada.
- talvez seja necessário iterar com o GitHub Copilot, solicitando alterações e fornecendo contexto. Esse é o fluxo normal de trabalho com o Copilot!

## Recursos

- [Copilot nos modos ask, edit e agent: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
