---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

Quando você usa o GitHub Copilot no modo de agente, ele trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho ocorre em tempo real na sua máquina, o Copilot Chat no VS Code mostra todas as chamadas de ferramentas e edições de arquivos à medida que acontecem. Você pode revisar cada diff no próprio editor, aceitar ou rejeitar alterações individuais e orientar a conversa com prompts adicionais para refinar ou ampliar o trabalho sem sair do editor.

Neste exercício, você vai:

- acompanhar o trabalho do agente de acessibilidade em tempo real.
- revisar e aceitar as edições propostas nos arquivos.
- orientar o agente durante a sessão para adicionar um controle do modo claro junto com o modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho está em andamento, a equipe de design faz outra solicitação: também quer um controle para o modo claro, para as pessoas que acham o tema padrão escuro demais. Em vez de começar uma nova conversa, você orientará a sessão existente para ampliar o trabalho.

## Acompanhe o progresso do Copilot

A exibição Copilot Chat mostra cada etapa do agente: os arquivos explorados, os comandos executados e as edições propostas. Cada edição aparece na conversa com um diff e os controles **Keep** e **Undo**.

1. Volte à exibição Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, percorra a conversa para acompanhar cada chamada de ferramenta. Você verá leituras e edições de arquivos e os comandos de terminal cuja execução for solicitada.
3. Quando o agente propuser uma edição, o diff aparecerá na conversa:
   - selecione **Keep** para aceitar a alteração.
   - selecione **Undo** para desfazê-la e deixar o agente tentar novamente.
4. Você também pode abrir o indicador **Files changed** na parte superior do chat para conferir todos os arquivos modificados pelo agente durante a sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão (por exemplo, para executar um comando no terminal), o agente fará uma pausa e mostrará o pedido **Allow**. Aprove somente os comandos que considerar seguros.

5. Aguarde até que o agente termine de implementar o modo de alto contraste. Você verá uma mensagem final que resume as alterações.

## Oriente o agente para adicionar o modo claro

Agora que o modo de alto contraste está implementado, você ampliará a mesma conversa para adicionar um controle do modo claro. Como continuará na mesma sessão de chat, o agente mantém todo o contexto do trabalho anterior, incluindo os componentes alterados, a abordagem de estilo e o padrão de armazenamento local.

1. No campo de entrada do Copilot Chat, envie o seguinte prompt:

   ```plaintext
   Bom trabalho! Agora adicione um controle para o modo claro junto com o controle de alto contraste. O modo claro deve seguir o mesmo padrão: persistir no armazenamento local e poder ser acessado na mesma área da interface. As pessoas usuárias devem poder ativar o alto contraste e o modo claro de forma independente.
   ```

2. O agente continuará o trabalho, explorando as alterações já feitas e propondo adições para o modo claro.
3. Revise cada nova edição quando aparecer, como fez com o modo de alto contraste. Use **Keep** ou **Undo** para cada diff.
4. Se o agente seguir uma direção indesejada — por exemplo, tentar refatorar os controles de uma maneira que você não queira —, pare-o com o botão **Stop** ao lado do campo de entrada e envie uma mensagem de esclarecimento.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que recomeçar. Use prompts adicionais para pedir ajustes ("simplifique o CSS — use variáveis CSS em vez de duplicar seletores"), corrigir o rumo ("não adicione um terceiro controle; integre o modo claro ao painel de configurações existente") ou ampliar o escopo ("adicione atalhos de teclado para os dois controles").

## Revise o conjunto de alterações

Antes de fazer commit do trabalho, revise rapidamente tudo o que foi alterado pelo agente.

1. Abra a exibição **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá ver atualizações nos componentes Astro, nos estilos e nos testes relacionados.
3. Abra alguns dos arquivos alterados e percorra os diffs. Confirme se seguem as práticas de acessibilidade do agente personalizado: atributos ARIA, navegação pelo teclado, HTML semântico e persistência no armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações no painel Source Control. Você as publicará [na próxima lição][next-lesson].

## Resumo e próximos passos

Você usou o modo de agente do Copilot Chat para acompanhar e orientar uma sessão em andamento: revisou cada edição conforme foi criada, aceitou ou rejeitou alterações e enviou prompts adicionais para ampliar o trabalho sem perder o contexto.

Neste exercício, você explorou:

- como acompanhar o trabalho de um agente em tempo real.
- como aceitar ou rejeitar edições individuais em arquivos.
- como orientar uma sessão em andamento com prompts adicionais.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade em um pull request e revisará as alterações feitas pelo agente personalizado local durante o laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Uso do modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
