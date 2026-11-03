---
title: "Lição 5 - Personalize e use uma skill quality-checks"
description: "Explore a skill quality-checks existente, personalize o formato do relatório e use-a para validar a filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Escrever código envolve mais do que apenas programar. Já validamos manualmente o funcionamento do código e usamos arquivos de instruções para garantir que ele siga os padrões. Mas e os testes? E o lint? E as outras partes da integração contínua (CI)?

Para essas tarefas, as **skills de agente** são ideais! As skills ajudam o Copilot a entender como executar operações desse tipo.

Nesta lição, você vai:

- explorar a skill `quality-checks` existente.
- personalizar o formato dos resultados.
- recarregar e executar a skill.

## Cenário

A Tailspin Toys usa a skill `quality-checks` para testes unitários, lint e verificações de tipos. A equipe quer melhorar o relatório para facilitar a leitura dos resultados.

## Instruções, scripts e recursos

As skills de agente reúnem instruções reutilizáveis para tarefas, scripts executáveis e recursos de apoio que um agente carrega sob demanda. Em essência, uma skill é uma pasta com o nome dela e um arquivo Markdown chamado `SKILL.md`. O Markdown contém frontmatter com o nome e a descrição da skill, uma visão geral do que ela faz e orientações sobre quando deve ser usada. A pasta também pode incluir subpastas com scripts e outros recursos que a skill utilizará.

> [!NOTE]
> Uma skill não precisa de pastas ou arquivos adicionais. A skill `quality-checks` da Tailspin Toys contém apenas `SKILL.md`, pois usa os comandos existentes do projeto.

As skills podem ficar na pasta `.github/skills` do projeto para se tornarem recursos do repositório, compartilhados e reutilizados pela equipe, ou na pasta de skills do usuário, `~/.copilot/skills`.

## Explore a skill

Vamos explorar a skill `quality-checks` criada pela equipe da Tailspin Toys para executar testes unitários, lint e verificações de tipos.

1. Volte ao Codespace. No editor do Codespaces, abra `.github/skills/quality-checks/SKILL.md`.
2. Leia `name` e `description` no início do arquivo. A descrição ajuda o Copilot a entender quando deve chamar a skill.
3. Leia as instruções e observe como elas orientam o Copilot durante os testes e o lint.
4. Observe que a skill ainda não tem uma seção **Results output formatting**.

## Execute a skill antes da alteração

As skills podem ser chamadas diretamente no Copilot CLI ou usando linguagem natural. Vamos pedir ao Copilot que execute as três verificações da skill.

1. Volte à conversa de filtragem no modo Interactive.
2. Use o seguinte prompt:

   ```plaintext
   Execute a skill quality-checks para os testes unitários, o lint e as verificações de tipos.
   ```

3. Observe o relatório final.

## Personalize o relatório

Queremos que o relatório indique o que foi executado, se foi aprovado e o que as ferramentas realmente informaram. Vamos atualizar a skill para gerar esse relatório.

1. Volte a `.github/skills/quality-checks/SKILL.md`.
2. Adicione a seguinte seção ao final do arquivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. O arquivo será salvo automaticamente.

## Execute a skill atualizada

Agora vamos ver a atualização em ação! O Copilot CLI pode recarregar as skills editadas sem reiniciar a conversa.

1. Digite:

   ```plaintext
   /skills reload
   ```

2. Use exatamente o mesmo prompt de antes:

   ```plaintext
   Execute a skill quality-checks para os testes unitários, o lint e as verificações de tipos.
   ```

3. Observe o relatório final e compare-o com o primeiro.

## Resumo e próximos passos

Você personalizou e usou uma skill de agente existente. Nesta lição, você:

- explorou a skill `quality-checks` para testes unitários, lint e verificações de tipos.
- personalizou o formato dos resultados.
- recarregou e executou a skill.

Essa alteração acompanhará a filtragem no PR do recurso. Em seguida, você permitirá que o Copilot interaja diretamente com o site [por meio do servidor MCP do Playwright][next-lesson].

## Mais exemplos de skills

Estes exemplos da comunidade são referências, não tarefas adicionais:

- [Especificação de Agent Skills][skill-spec]
- [Fluxo de contribuição: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas e script de exportação incluído: `drawio`][drawio-example]
- [Testes de navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
