---
slug: pt-br/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Desenvolva, verifique e entregue duas alterações na Tailspin Toys enquanto aprende sobre modos, personalizações, ferramentas MCP e automação de pull requests no Copilot CLI."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O **[GitHub Copilot CLI][about-copilot-cli]** disponibiliza o GitHub Copilot no terminal como um assistente de programação baseado em agentes. Ele explora bases de código, gera código, executa comandos e se conecta a ferramentas externas — tudo pela linha de comando, para que você mantenha o foco sem alternar para um editor gráfico.

O workshop acompanha um fluxo de trabalho contínuo na Tailspin Toys:

1. Prepare o projeto no GitHub Codespaces, instale o Copilot CLI e conheça o ambiente.
2. Faça uma alteração específica nas avaliações com estrelas, revise-a no navegador e mescle manualmente seu primeiro pull request (PR).
3. Comece pela issue de filtragem, defina a abordagem no modo Plan, implemente-a no modo Autopilot e revise-a no modo Interactive.
4. Atualize as instruções do repositório e aplique-as ao trabalho de filtragem.
5. Personalize a skill `quality-checks` existente e use-a para executar as verificações do projeto.
6. Adicione o servidor Model Context Protocol (MCP) do Playwright e use-o para explorar a filtragem no navegador.
7. Crie um agente personalizado de garantia de qualidade (QA) e use-o para revisar requisitos, cobertura e evidências de verificação.
8. Revise todas as alterações da filtragem e use o Agent Merge para o PR do recurso.
9. Explore comandos slash úteis para contexto, modelos, compartilhamento e delegação opcional à nuvem.

Para manter o workshop focado, você criará dois PRs: primeiro, o das avaliações com estrelas; depois, o da filtragem, que incluirá atualizações das instruções, atualização da skill, perfil de QA e testes. O fluxo de filtragem e qualidade compartilha uma conversa e um branch para que você desenvolva o trabalho à medida que explora cada ferramenta.

## Lições

| Lição | Tópico | Descrição |
| ------ | ----- | ----------- |
| [0. Pré-requisitos][ex0] | Configuração | Crie seu repositório e Codespace |
| [1. Instale o Copilot CLI][ex1] | Instalação | Instale e autentique o Copilot CLI e conheça o ambiente |
| [2. Adicione avaliações com estrelas: uma vitória rápida][ex2] | Primeira alteração | Exiba as avaliações existentes e o estado alternativo para valores nulos; depois, mescle seu primeiro PR |
| [3. Modos de agente: Plan e Autopilot][ex3] | Modos de agente | Planeje o recurso com base na issue, implemente-o com o Autopilot e revise-o no modo Interactive |
| [4. Oriente o Copilot com instruções personalizadas][ex4] | Contexto | Explore e atualize as instruções e aplique-as à filtragem |
| [5. Personalize e use uma skill quality-checks][ex5] | Verificações repetíveis | Explore a skill existente, altere o formato do relatório e execute-a |
| [6. Valide a funcionalidade com Playwright MCP][ex6] | Observação no navegador | Configure o MCP no CLI e inspecione o comportamento da filtragem |
| [7. Crie e use um agente de QA][ex7] | Requisitos e cobertura | Crie e selecione um perfil especializado e reúna as evidências finais de verificação |
| [8. Crie e mescle o PR do recurso][ex8] | Revisão e mesclagem | Revise todas as alterações, crie o PR e use o Agent Merge |
| [9. Comandos slash do GitHub Copilot CLI][ex9] | Recursos do CLI | Explore o contexto, os modelos, o compartilhamento e a delegação opcional ao agente de nuvem |
| [10. Encerramento e próximos passos][ex10] | Resumo | Revise o fluxo de trabalho, as personalizações reutilizáveis e outros recursos |
| [Opcional: Incorpore o Foundry][foundry] | Agentes hospedados | Prepare um modelo, implante um agente baseado no catálogo e conecte-o ao site |

## Pré-requisitos

Antes de participar deste workshop, verifique se você tem:

- [ ] Uma conta do GitHub com um plano **Copilot Student, Pro, Pro+, Business ou Enterprise** ativo
- [ ] Permissão para criar um repositório e um Codespace
- [ ] Familiaridade básica com o terminal ou operações de linha de comando

> [!TIP]
> Não tem um plano pago? Estudantes com status verificado podem obter o GitHub Copilot gratuitamente pelo [GitHub Education][student-plan]. O plano **Copilot Student** inclui o agente, MCP, revisão de código e os recursos do Copilot CLI usados neste workshop.

> [!NOTE]
> Se você usa o Copilot Business ou o Copilot Enterprise, confirme se o administrador habilitou o Copilot CLI.

## Comece

**[Comece pelos pré-requisitos →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
