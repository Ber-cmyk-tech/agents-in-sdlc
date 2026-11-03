---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que poucas — se é que alguma — organizações não enfrentem problemas com dívida técnica. Pode ser o caso de problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de funcionalidades esquecidas no backlog por falta de tempo para implementá-las. O agente de nuvem do GitHub Copilot foi criado para executar tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina, ele gera um rascunho de PR para revisão por uma pessoa desenvolvedora. Assim, é possível delegar tarefas tediosas, acelerar o desenvolvimento e liberar as pessoas desenvolvedoras para se concentrarem em questões mais importantes.

Neste exercício, você explorará o agente de nuvem do Copilot para:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam realizadas com segurança.
- entender a importância de delimitar as issues com clareza.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas dívidas técnicas que gostaria de resolver. As pessoas contratadas inicialmente para criar a primeira versão do site deixaram a documentação em um estado inadequado — na verdade, você verá que ela está completamente ausente. Como primeiro passo, a equipe quer adicionar comentários TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design está pronta para melhorar a descoberta de jogos. Ela quer mostrar jogos relacionados — outros títulos da mesma categoria — na página de detalhes de cada jogo para que as pessoas continuem explorando. Ainda não é necessária uma implementação refinada; basta algo que permita fazer testes de aceitação da experiência do usuário. Essa tarefa está bloqueando o trabalho, mas há outras issues de prioridade mais alta no momento.

Esses são exemplos de tarefas que podem ser adiadas facilmente e que são ótimas candidatas para atribuição ao agente de nuvem do Copilot. Ele pode trabalhar nelas de forma assíncrona, permitindo que a pessoa desenvolvedora se concentre em outras tarefas e depois volte para revisar o trabalho do Copilot e confirmar se tudo está conforme o esperado.

## Apresentação do agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano, de maneira semelhante a uma pessoa desenvolvedora. Assim como no trabalho com outra pessoa desenvolvedora, há várias maneiras de iniciar uma tarefa, inclusive [atribuindo uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois que a issue é atribuída, o Copilot cria um rascunho de pull request para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode examinar a sessão enquanto o trabalho está em andamento ou depois que ele terminar. Quando a solução proposta estiver pronta para sua revisão, o Copilot mencionará você no pull request.

## A importância de delimitar bem as instruções

O GitHub Copilot não faz mágica — você ainda precisa pensar. Até operações aparentemente simples podem se tornar complexas quando examinadas em detalhes. Por isso, [defina com cuidado o escopo das tarefas para o agente de nuvem do Copilot][cloud-agent-best-practices]. Trate-o como uma pessoa parceira de programação com IA: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a inclusão da IA generativa.

## Instruções personalizadas neste repositório

Nos exercícios anteriores, você conheceu as instruções personalizadas e como elas orientam o Copilot. Se já concluiu o exercício prático sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação; caso contrário, este é um bom momento para dar uma olhada. Antes de atribuir trabalho ao agente de nuvem do Copilot, leia os arquivos de instruções incluídos neste repositório sem fazer alterações, para que depois você possa perceber a influência deles.

Abra os seguintes arquivos na interface Web do GitHub ou em um codespace, se já houver um em execução:

- `.github/copilot-instructions.md` — revise a seção **Code standards**, principalmente as expectativas sobre convenções de TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que aplica as instruções aos arquivos `**/*.test.ts`.

Na próxima seção, quando atribuir a issue `Code lacks documentation` ao agente de nuvem, observe os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de comentários no pull request gerado — eles são definidos por esses arquivos de instruções. Voltaremos a isso ao revisar PRs em um exercício posterior.

## Configure o ambiente de desenvolvimento do agente de nuvem do Copilot

A criação de código costuma exigir um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja pronto, independentemente de quem esteja desenvolvendo. O mesmo vale para as tarefas atribuídas ao Copilot, que trabalha de maneira semelhante a uma pessoa engenheira de software.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizar esse ambiente criando um [fluxo de trabalho de configuração especial][setup-workflow] no arquivo `.github/workflows/copilot-setup-steps.yml`, que será executado antes de o agente começar a trabalhar. Isso dá acesso às ferramentas de desenvolvimento e dependências necessárias. A configuração já foi preparada para simplificar o laboratório e facilitar o aprendizado. Ela fornece ao Copilot o Node.js, as dependências do projeto, os navegadores do Playwright para testes de ponta a ponta e o banco de dados SQLite migrado e preenchido com dados para o aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões no nível mínimo necessário para *suas etapas*. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Se você quiser clonar o repositório como parte da configuração, por exemplo, para instalar dependências, precisará da permissão `contents: read`. Se não clonar o repositório nas etapas de configuração, o Copilot fará isso automaticamente quando elas terminarem.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend/aplicativo: Node.js (o aplicativo agora usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migre e preencha o banco de dados SQLite local para que os builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo parece qualquer outro fluxo de trabalho do GitHub, mas há alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`. Esse job é executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- Observe o gatilho `workflow_dispatch`, que permite executar o fluxo manualmente na guia **Actions** do repositório. Isso é útil para confirmar que o fluxo funciona sem precisar esperar o Copilot executá-lo.

## Adicione documentação

Embora todo mundo reconheça a importância da documentação, muitos projetos têm informações desatualizadas ou não têm documentação alguma. Esse tipo de dívida técnica muitas vezes fica sem solução, reduzindo a produtividade e dificultando a manutenção da base de código ou a integração de novas pessoas desenvolvedoras. Felizmente, o Copilot é muito bom em criar documentação, e esta é uma issue perfeita para atribuir ao agente de nuvem. Ele trabalhará em segundo plano para gerar a documentação necessária. Em um exercício futuro, você voltará para revisar o resultado.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para criar uma issue em branco.
5. Em **Title**, digite `Code lacks documentation`.
6. Em **Description**, digite:

   ```plaintext
   Nossa organização exige que funções e métodos tenham comentários TSDoc quando eles forem úteis. Infelizmente, as atualizações recentes não seguiram esse padrão. Precisamos atualizar o código existente para incluir comentários de documentação nos pontos em que esclareçam o comportamento.
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

   ![Detalhes da atribuição da issue ao Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) recém-gerado, cujo título será algo como `[WIP]: Code lacks documentation`. Se ele ainda não aparecer na lista, aguarde um ou dois minutos e atualize a janela do navegador.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas do Copilot pode levar alguns minutos para aparecer no PR. O Copilot está configurando o ambiente (executando o fluxo de trabalho descrito anteriormente), analisando o projeto e definindo a melhor abordagem para a tarefa.

13. Revise a lista e as tarefas que serão executadas.
14. Role a linha do tempo do pull request para baixo. Você verá uma atualização indicando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

   ![Exibição da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe que você pode percorrer a sessão em tempo real e acompanhar como o Copilot resolve o problema: ele explora o código e entende o estado atual, faz uma pausa para pensar e decidir sobre a abordagem adequada e, em seguida, cria o código.

Isso provavelmente levará alguns minutos. Um dos principais objetivos do agente de nuvem do Copilot é executar tarefas de forma assíncrona, liberando você para se concentrar em outras atividades. Vamos aproveitar esse recurso atribuindo outra tarefa ao agente de nuvem e, em seguida, voltando nossa atenção à criação de funcionalidades para o aplicativo.

## Adicione uma seção de jogos relacionados à página de detalhes

Como já destacamos, uma das grandes vantagens do agente de nuvem do GitHub Copilot é a possibilidade de dividir o trabalho: você pode se concentrar em um conjunto de tarefas enquanto ele trabalha em outro. Mesmo que a seção de jogos relacionados solicitada pela equipe de design não leve muito tempo, esse tempo poderia ser usado em outras tarefas. Vamos atribuí-la ao agente de nuvem do Copilot!

1. Volte ao seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para usar o modelo em branco.
5. Em **Title**, digite `Show related games on the game details page`.
6. Em **Description**, digite:

   ```markdown
   Queremos ajudar as pessoas a descobrir mais jogos, exibindo jogos relacionados na página de detalhes de cada jogo. A equipe de design quer explorar a experiência do usuário e fazer testes de aceitação. Os requisitos são:

   - Adicione um auxiliar de acesso a dados em `src/lib/` que retorne outros jogos da mesma categoria de determinado jogo, excluindo o próprio jogo.
   - Mostre uma seção "Jogos relacionados" na página de detalhes do jogo usando o auxiliar.
   - Trate o caso em que não há jogos relacionados.
   - Crie testes unitários para o novo auxiliar.
   - Antes de criar o PR, confirme que todos os testes foram aprovados.
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Em pouco tempo, você verá um conjunto de 👀 no primeiro comentário da issue, indicando que o Copilot começou a trabalhar!

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot já está trabalhando diligentemente na segunda solicitação! O agente de nuvem do Copilot trabalha de forma semelhante a uma pessoa engenheira de software. Portanto, você não precisa monitorar o trabalho ativamente; basta revisá-lo quando estiver concluído. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Nesta lição, você explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com ele, você pode atribuir issues ao Copilot para que sejam executadas de forma assíncrona. Use-o para resolver dívidas técnicas, criar funcionalidades ou ajudar a migrar código entre frameworks.

Você explorou estes conceitos:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam realizadas com segurança.
- entender a importância de delimitar as issues com clareza.
- atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha, vamos criar e usar agentes personalizados. O [agente de nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e receber instruções personalizadas, recursos que exploramos nos módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
