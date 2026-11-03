---
title: "Lección 7 - Crear y usar un agente de QA"
description: "Crea un agente personalizado de QA que combine los requisitos de la incidencia, la habilidad quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has usado la habilidad `quality-checks` para ejecutar comprobaciones automatizadas y Playwright MCP para observar la experiencia de filtrado en un navegador. Ahora combinarás esas capacidades en un agente personalizado con un proceso de QA bien definido.

En esta lección:

- explorarás cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP.
- crearás y revisarás un perfil reutilizable de aseguramiento de calidad.
- seleccionarás el agente de QA y revisarás sus conclusiones con respecto a la incidencia del filtrado.

## Escenario

Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de pruebas y el comportamiento en el navegador antes de abrir una solicitud de incorporación de cambios (PR). Un agente personalizado puede coordinar el proceso de QA y generar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot definida en un perfil de Markdown. El perfil describe el propósito, las instrucciones y las herramientas disponibles para el agente. En este taller definirás el rol de QA en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Cada personalización cumple una función distinta: las instrucciones del repositorio describen los estándares del equipo; la habilidad `quality-checks` agrupa comprobaciones repetibles; Playwright MCP proporciona herramientas de navegador; y el perfil de QA indica a Copilot cómo usar esas capacidades para comprobar los requisitos e informar de los resultados.

## Crea el perfil de QA

Antes de abrir la PR de la función, pide a Copilot que cree un perfil de QA reutilizable. El perfil definirá las comprobaciones y los límites que debe respetar.

1. Vuelve a tu Codespace y confirma que la conversación de filtrado está en modo Interactive.
2. Pide al agente predeterminado que cree el agente:

   ```plaintext
   Crea un agente personalizado llamado QA en .github/agents/qa.agent.md. Debe comprobar las funciones con respecto a las incidencias y los requisitos acordados, seguir las instrucciones del repositorio, ejecutar la habilidad quality-checks, usar Playwright MCP para verificar el comportamiento y añadir pruebas si falta cobertura.

   Debe informar de cada requisito como superado, fallido o bloqueado y aportar pruebas. Debe pedirme permiso antes de cambiar el código de implementación y no debe confirmar cambios ni abrir solicitudes de incorporación de cambios.

   Por ahora, crea únicamente el perfil para que pueda revisarlo.
   ```

## Inspecciona el perfil

Revisa el perfil para confirmar que Copilot ha incluido el flujo de QA y sus límites.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee el frontmatter. `description` es obligatorio; `name` es opcional, pero ayuda a identificar el agente.
3. Confirma que el perfil empieza por los requisitos, sigue las instrucciones del repositorio, ejecuta `quality-checks` y usa Playwright MCP.
4. Confirma que presenta pruebas, pide permiso antes de modificar la implementación y no confirma cambios ni abre PR.
5. Si falta alguna responsabilidad o límite, pide al agente predeterminado que revise el perfil antes de continuar.

## Ejecuta QA con respecto a la incidencia

Copilot CLI carga los agentes del proyecto al iniciar una conversación. Reanuda la conversación de filtrado, selecciona QA y aprovecha el contexto de la incidencia y de las decisiones de planificación.

1. Activa el agente con este comando:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, es posible que no aparezca en la lista de agentes disponibles. Aun así, el comando anterior lo activará.

2. Pide al agente de QA que revise la función:

   ```plaintext
   Revisa la función de filtrado con respecto a la incidencia y las decisiones del plan. ¿Está lista para una PR?
   ```

3. Lee el informe cuando termine.

## Resumen y siguientes pasos

Has añadido un rol especializado reutilizable y has revisado su trabajo. En esta lección:

- has explorado cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas.
- has creado y revisado un perfil de QA.
- has seleccionado el agente de QA y revisado sus conclusiones con respecto a la incidencia.

Ya tienes la implementación, la habilidad actualizada, el perfil de QA, las pruebas y el informe de validación listos para revisión. A continuación, [los reunirás en una PR de la función y usarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
