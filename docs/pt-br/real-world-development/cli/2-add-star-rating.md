---
title: "Lição 2 - Adicione avaliações com estrelas: uma vitória rápida"
description: "Use o Copilot CLI para fazer uma pequena alteração nos cards de jogos, revise-a em um navegador encaminhado e mescle-a como seu primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou o Copilot CLI e experimentou uma conversa, é hora de fazer sua primeira alteração no projeto. Ela será pequena: os dados dos jogos já incluem uma avaliação com estrelas, mas os cards da página inicial ainda não a mostram. Você pedirá ao agente que a exiba, revisará a alteração e a mesclará como seu primeiro pull request.

Nesta lição, você vai:

- iniciar uma conversa focada do Copilot em um branch de funcionalidade.
- pedir ao agente que faça uma pequena alteração no projeto.
- revisar as alterações com `/diff`.
- executar o aplicativo para confirmar a alteração em um navegador encaminhado.
- abrir e mesclar seu primeiro pull request.

## Cenário

Cada jogo na Tailspin Toys pode ter uma avaliação com estrelas, que já aparece na página de detalhes do jogo. Os cards da página inicial, porém, mostram apenas o título, a categoria, a editora e a descrição. Como aquecimento, você pedirá ao agente que exiba a avaliação existente em cada card — uma alteração pequena e independente, perfeita para sua primeira sessão.

## Estrutura de uma conversa

Uma **conversa** é onde você trabalha com o Copilot CLI em uma tarefa. Diferentemente do aplicativo Copilot, uma conversa normal no CLI usa o repositório e o branch do Git atualmente selecionados no terminal, em vez de criar um worktree dedicado. As conversas salvas permitem retomar a mesma discussão depois, enquanto os arquivos e o branch permanecem como estado comum do Git no disco.

Em uma conversa, você verá três coisas: seus prompts e as respostas do agente, a atividade das ferramentas do agente enquanto ele explora e edita arquivos e as alterações que você pode inspecionar com `/diff`.

## Inicie uma conversa e solicite a alteração

Vamos iniciar uma nova conversa para começar a implementar o recurso.

1. Volte ao Codespace.
2. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Se o Copilot ainda não estiver em execução, inicie-o com o seguinte comando:

   ```bash
   copilot --yolo
   ```

4. Inicie uma nova sessão com o comando slash `/new` e pressione <kbd>Enter</kbd>.
5. Use este prompt para solicitar a alteração:

   ```plaintext
   Mostre a avaliação starRating de cada jogo, de 0 a 5, nos cards da lista de jogos. Se a avaliação for null, mostre "No rating yet". Mantenha o layout dos cards, adicione testes e execute as verificações pertinentes.
   ```

O Copilot explorará o projeto, localizará os arquivos usados para exibir os detalhes dos jogos e criará o código necessário. Você acabou de adicionar um recurso com o Copilot CLI!

## Revise as diferenças

Todas as alterações geradas por IA devem ser revisadas antes da mesclagem, mesmo as pequenas. Vamos explorar as alterações diretamente no Copilot CLI.

1. Digite `/diff` e inspecione todos os arquivos alterados.
2. Confirme que o card do jogo exibe a avaliação numérica quando ela está disponível e `No rating yet` quando `starRating` é `null`.
3. Confirme que os testes cobrem os dois casos.
4. Revise os resultados das verificações executadas pelo Copilot e peça que ele corrija eventuais falhas.
5. Quando terminar a revisão, pressione <kbd>Esc</kbd> para sair da tela de diferenças.

> [!NOTE]
> Como todas as ferramentas de IA generativa, o Copilot é probabilístico, não determinístico, então o código exato pode variar. Revise o comportamento em vez de esperar uma implementação específica.

## Confira as alterações

Não devemos apenas ler o código e presumir que funciona. Vamos pedir ao Copilot que inicie o site para que possamos examinar a interface do usuário (UI) atualizada no navegador encaminhado pelo Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Inicie o aplicativo para que eu possa inspecionar no navegador a alteração da avaliação com estrelas. Informe a URL e deixe o servidor em execução.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme que os cards dos jogos exibem as avaliações de zero a cinco.
4. Volte ao Copilot e peça que ele pare o servidor iniciado:

   ```plaintext
   Pare o servidor de desenvolvimento que você iniciou.
   ```

## Abra e mescle seu primeiro pull request

Você criou o recurso! Agora é hora de abrir um pull request (PR) para mesclar o novo código ao projeto.

1. Peça ao agente padrão que faça o commit da alteração:

   ```plaintext
   Faça o commit das alterações revisadas na avaliação com estrelas, usando uma mensagem de commit adequada.
   ```

2. Digite `/pr create`. O Copilot CLI enviará o commit existente ao criar o PR e exibirá a URL dele.
3. Abra o PR mantendo <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) pressionado e selecionando a URL exibida pelo Copilot CLI.
4. Revise os arquivos alterados e as verificações.
5. Quando estiver tudo pronto, selecione **Merge pull request** e confirme a mesclagem.
6. Volte ao Codespace e saia do Copilot CLI com `/exit`.
7. Atualize seu `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximos passos

Parabéns! Você entregou sua primeira alteração usando o GitHub Copilot CLI! Especificamente, você:

- iniciou uma conversa focada do Copilot em um branch de funcionalidade.
- orientou o agente a fazer uma pequena alteração nos cards dos jogos.
- revisou as alterações com `/diff`.
- executou o aplicativo para confirmar a avaliação com estrelas em um navegador encaminhado.
- abriu e mesclou seu primeiro pull request.

Em seguida, você [partirá da issue de filtragem e usará os modos Plan e Autopilot][next-lesson] para criar um recurso maior.

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
