---
slug: es-es/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Desarrolla, verifica y entrega dos cambios de Tailspin Toys mientras aprendes los modos, las personalizaciones, las herramientas MCP y la automatización de solicitudes de incorporación de cambios de Copilot CLI."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** incorpora GitHub Copilot a tu terminal como asistente de programación con agentes. Explora bases de código, genera código, ejecuta comandos y se conecta a herramientas externas, todo desde la línea de comandos, para que puedas mantener el flujo sin cambiar a un editor gráfico.

El taller sigue un flujo continuo de Tailspin Toys:

1. Prepararás el proyecto en GitHub Codespaces, instalarás Copilot CLI y explorarás el repositorio.
2. Añadirás valoraciones con estrellas, revisarás el resultado en un navegador reenviado y fusionarás manualmente tu primera solicitud de incorporación de cambios (PR).
3. Partirás de la incidencia de filtrado, definirás el enfoque en el modo Plan, lo implementarás en Autopilot y lo revisarás con Interactive.
4. Actualizarás las instrucciones del repositorio y las aplicarás al trabajo de filtrado.
5. Personalizarás la habilidad `quality-checks` existente y la usarás para ejecutar las comprobaciones del proyecto.
6. Añadirás el servidor Playwright del Protocolo de contexto de modelo (MCP) y lo usarás para explorar el filtrado en un navegador.
7. Crearás un agente personalizado de aseguramiento de calidad (QA) para revisar los requisitos, la cobertura y las pruebas de verificación.
8. Revisarás todos los cambios del filtrado y usarás Agent Merge para fusionar su PR.
9. Explorarás comandos de barra para el contexto, los modelos, el uso compartido y la delegación opcional en la nube.

Para mantener el taller centrado, crearás dos PR: una para las valoraciones con estrellas y otra para el filtrado, con las actualizaciones de instrucciones, la habilidad, el perfil de QA y las pruebas. El flujo de filtrado y calidad comparte una conversación y una rama para que puedas aprovechar el trabajo al explorar cada herramienta.

## Lecciones

| Lección | Tema | Descripción |
|----------|-------|-------------|
| [0. Requisitos previos][ex0] | Configuración | Crea el repositorio y el codespace |
| [1. Instalar Copilot CLI][ex1] | Instalación | Instala y autentica Copilot CLI; después, familiarízate con la herramienta |
| [2. Añadir valoraciones con estrellas: una mejora rápida][ex2] | Primer cambio | Muestra las valoraciones existentes y el estado sin valoración; después, fusiona tu primera PR |
| [3. Modos de agente: Plan y Autopilot][ex3] | Modos de agente | Planifica la función desde su incidencia, créala con Autopilot y revísala con Interactive |
| [4. Guiar a Copilot con instrucciones personalizadas][ex4] | Contexto | Explora y actualiza las instrucciones; después, aplícalas al filtrado |
| [5. Personalizar y usar una habilidad quality-checks][ex5] | Comprobaciones repetibles | Explora la habilidad existente, cambia el formato del informe y ejecútala |
| [6. Validar la funcionalidad con Playwright MCP][ex6] | Observación en el navegador | Configura MCP en la CLI e inspecciona el comportamiento del filtrado |
| [7. Crear y usar un agente de QA][ex7] | Requisitos y cobertura | Crea y selecciona un perfil especializado y reúne las pruebas de verificación |
| [8. Crear y fusionar la PR de la función][ex8] | Revisión y fusión | Revisa todos los cambios, crea la PR y usa Agent Merge |
| [9. Comandos de barra en GitHub Copilot CLI][ex9] | Funciones de CLI | Explora el contexto, los modelos, el uso compartido y la delegación opcional en la nube |
| [10. Resumen y siguientes pasos][ex10] | Resumen | Repasa el flujo de trabajo, las personalizaciones reutilizables y otros recursos |
| [Opcional: incorporar Foundry][foundry] | Agentes hospedados | Prepara un modelo, despliega un agente basado en el catálogo y conéctalo al sitio web |

## Requisitos previos

Antes de asistir a este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan activo de **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] Permiso para crear un repositorio y un codespace
- [ ] Conocimientos básicos de operaciones de terminal/línea de comandos

> [!TIP]
> ¿No tienes un plan de pago? Si eres estudiante verificado, puedes obtener GitHub Copilot gratis a través de [GitHub Education][student-plan]. El plan **Copilot Student** incluye las funciones de agente, MCP, revisión de código y Copilot CLI que usa este taller.

[student-plan]: https://github.com/education/students

> [!NOTE]
> Si usas Copilot Business o Copilot Enterprise, asegúrate de que tu administrador haya habilitado Copilot CLI.

## Empieza

**[Empieza con los requisitos previos →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
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
