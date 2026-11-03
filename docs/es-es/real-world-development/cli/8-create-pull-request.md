---
title: "Lección 8 - Crear y fusionar la PR de la función"
description: "Revisa el filtrado, las instrucciones, la habilidad actualizada, el perfil de QA y las pruebas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones, la habilidad actualizada, el perfil de aseguramiento de calidad (QA) y las pruebas están guardados en una misma rama. Ahora revisarás los cambios y abrirás una solicitud de incorporación de cambios. Fusionaste manualmente la PR de las valoraciones con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la función, las actualizaciones de instrucciones, la habilidad y el agente de QA en varias PR. Para agilizar el taller, has mantenido el flujo completo de filtrado y calidad en una conversación y una rama.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- inspeccionarás los cambios completos de la función y las pruebas de verificación.
- crearás la PR del filtrado.
- activarás Agent Merge solo después de revisar los cambios y confirmarás la fusión.

## Escenario

Has usado Copilot para planificar, implementar y verificar una función. Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener bajo control del desarrollador la autorización para fusionar.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios. Al habilitarlo, Copilot trabaja para resolver los impedimentos de la PR: corrige las comprobaciones de integración continua (CI) fallidas, responde a los comentarios de revisión y vuelve a aplicar los cambios cuando sea necesario. Después, habilita la fusión automática de GitHub si la configuración del repositorio lo permite.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de esa tarea, pero debes revisar el trabajo y confirmar que está listo antes de activarlo.

## Usa Agent Merge para gestionar la PR

1. Vuelve a tu Codespace.
2. Abre el diálogo de agentes con `/agent`.
3. Selecciona **Default** y pulsa <kbd>Enter</kbd>.
4. Crea una PR con `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará la integración continua de la PR y la fusionará cuando todo se complete correctamente.
7. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de fusión, la configuración del repositorio ni los permisos que falten. Si el proceso se bloquea, revisa el motivo indicado y completa manualmente la fusión revisada cuando el repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: generación de código, pruebas y validación, y ahora la gestión de la PR. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- has inspeccionado los cambios completos de la función y las pruebas de verificación.
- has creado la PR del filtrado.
- has activado Agent Merge solo después de revisar los cambios y has confirmado la fusión.

A continuación, [explorarás más comandos de barra útiles de Copilot CLI][next-lesson] para gestionar contexto, modelos, sesiones compartidas y, opcionalmente, delegar trabajo en la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
