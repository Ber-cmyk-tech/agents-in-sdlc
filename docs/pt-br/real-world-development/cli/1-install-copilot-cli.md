---
title: "Lição 1 - Instale o GitHub Copilot CLI"
description: "Instale e autentique o Copilot CLI no Codespace, conheça o ambiente e localize a issue de filtragem já criada."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O [GitHub Copilot CLI][about-copilot-cli] é um poderoso assistente de programação baseado em agentes que funciona no terminal. Ele permite explorar bases de código, gerar código, executar comandos e interagir com ferramentas externas — tudo pela linha de comando. Você pode delegar tarefas, solicitar alterações e manter o foco. Como você deve imaginar, o primeiro passo é instalar a ferramenta. Felizmente, isso pode ser feito com ferramentas que você já conhece.

Nesta lição, você vai:

- instalar o GitHub Copilot CLI com npm.
- autenticar sua conta do GitHub.
- confiar no repositório do workshop e iniciar uma conversa rápida.
- localizar a issue de filtragem pelo servidor MCP integrado do GitHub.

## Cenário

Sua equipe está começando a usar agentes de IA para dar conta de um backlog crescente. O Copilot CLI leva esses recursos ao terminal, onde muitas pessoas desenvolvedoras já trabalham. Nesta lição, você instalará e autenticará a ferramenta e se preparará para usá-la no restante do workshop.

## Instale o Copilot CLI

Você pode instalar o Copilot CLI com [npm][install-cli], WinGet ou Homebrew. Como o GitHub Codespaces já inclui o Node.js, usaremos o npm.

1. Volte ao Codespace e abra um terminal.
2. Confirme se o Node.js está instalado e atende ao requisito de versão:

   ```bash
   node --version
   ```

   A versão deve ser 24 ou posterior.

3. Instale o Copilot CLI globalmente:

   ```bash
   npm install -g @github/copilot
   ```

4. Verifique a instalação:

   ```bash
   copilot --version
   ```

   O número da versão será exibido.

## Autentique-se no GitHub

Na primeira execução, o Copilot CLI solicitará que você autentique sua conta do GitHub.

1. Inicie o Copilot CLI:

   ```bash
   copilot
   ```

2. Se solicitado, siga as instruções de autenticação com código do dispositivo e autorize o Copilot CLI.
3. O Copilot CLI exibirá este prompt:

   ```plaintext
   O Copilot pode ler arquivos desta pasta e, com sua permissão, editá-los ou executar código e comandos do shell. Ele lembrará suas permissões durante o restante desta sessão.

   Você confia nos arquivos desta pasta?
   ```

4. Confirme que o caminho corresponde ao repositório Tailspin Toys e responda que sim selecionando **Yes, and remember this folder for future sessions**.

> [!NOTE]
> Em um Codespace, talvez você já esteja autenticado pela sessão do GitHub. Se o Copilot CLI iniciar sem pedir autenticação, está tudo certo!

## Conheça o ambiente

Os comandos digitados no prompt normal do shell são executados diretamente no Codespace. Depois que o Copilot CLI é iniciado, a linguagem natural é enviada ao agente e os comandos slash controlam a conversa.

1. Digite `/model`, use as teclas de seta para selecionar **Auto**, pressione <kbd>Enter</kbd> e, em seguida, pressione <kbd>Enter</kbd> novamente para confirmar.
2. Digite `/help` para ver os comandos disponíveis na versão instalada e pressione <kbd>Esc</kbd> para fechar a ajuda.
3. Faça uma pergunta simples ao Copilot para confirmar que tudo está funcionando:

   ```plaintext
   Quais são os principais arquivos deste projeto?
   ```

4. Leia a resposta e observe como o Copilot explora o repositório antes de responder.
5. Digite `/mcp list` e confirme que o servidor MCP integrado do GitHub está disponível.
6. Peça ao Copilot que encontre a issue de filtragem:

   ```plaintext
   Usando o GitHub MCP, encontre neste repositório a issue intitulada "Allow users to filter games by category and publisher". Informe a URL e um breve resumo. Não faça alterações.
   ```

7. Abra a URL e leia a issue. Você a usará depois de fazer uma primeira alteração rápida.

> [!TIP]
> Uma sessão normal do Copilot CLI trabalha no branch selecionado no terminal; ela não cria um worktree automaticamente. Você criará um branch de funcionalidade antes de cada alteração.

## Use o atalho do workshop

Normalmente, o Copilot CLI pede autorização antes de usar ferramentas fora das permissões já concedidas. Neste workshop, você o iniciará novamente com `--yolo`, um atalho autorizado pela pessoa usuária que remove essas solicitações de aprovação dentro do Codespace para que você possa se concentrar nos exercícios.

> [!CAUTION]
> `--yolo` habilita permissões totalmente automáticas (`--allow-all-tools`, `--allow-all-paths` e `--allow-all-urls`). Use-o somente em um ambiente isolado, como um Codespace ou uma VM, e nunca defina um alias padrão com essa opção para o desenvolvimento diário. Consulte [Permitir e negar o uso de ferramentas][allow-all-warning] para saber mais.

Neste workshop, `--enable-all-github-mcp-tools` habilita as ferramentas MCP do GitHub para leitura e gravação usadas nas lições seguintes para trabalhar com issues e pull requests. O Codespace restringe o acesso ao computador local, mas os recursos autenticados do GitHub continuam sendo reais. Revise as alterações antes de publicá-las ou mesclá-las.

1. Saia do Copilot CLI com `/exit`.
2. Inicie-o novamente na raiz do repositório:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Faça outra pergunta rápida sobre o projeto para confirmar que a conversa está funcionando e, em seguida, saia com `/exit`.

O Copilot salva as conversas automaticamente. Mais adiante, depois de alterar instruções ou adicionar um agente, você usará `copilot --resume` para voltar à mesma conversa e ao mesmo branch.

## Resumo e próximos passos

Parabéns! Nesta lição, você:

- instalou o GitHub Copilot CLI com npm.
- autenticou sua conta do GitHub.
- confiou no repositório do workshop e iniciou uma conversa rápida.
- localizou a issue de filtragem pelo servidor MCP integrado do GitHub.

Em seguida, você [fará sua primeira alteração específica][next-lesson] e usará o Copilot CLI para exibir uma avaliação com estrelas nos cards dos jogos.

## Recursos

- [Instalar o GitHub Copilot CLI][install-cli]
- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
