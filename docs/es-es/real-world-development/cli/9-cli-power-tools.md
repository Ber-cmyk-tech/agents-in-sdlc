---
title: "Lección 9 - Comandos de barra en GitHub Copilot CLI"
description: "Explora comandos de barra para gestionar el contexto, seleccionar modelos, compartir sesiones y delegar trabajo opcionalmente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier buena herramienta de CLI, GitHub Copilot CLI incluye numerosos comandos de barra para interactuar con ella. Estos comandos ofrecen funciones avanzadas, información interna y opciones de configuración. Ya has usado `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Ahora explorarás otros comandos útiles.

En esta lección:

- usarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- usarás `/model` para explorar los modelos disponibles.
- aprenderás cómo `/share` puede exportar o compartir una sesión.
- explorarás comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas en el agente en la nube.

## Escenario

Has terminado el flujo de trabajo principal de CLI. Ahora explorarás otras funciones: gestión del contexto, cambio de modelo, intercambio de sesiones y delegación opcional en el [agente de Copilot en la nube][about-cloud-agent].

## Explora el contexto de Copilot CLI

En tareas grandes o complejas puedes acercarte al límite de contexto del modelo. Copilot CLI compacta automáticamente la conversación cuando es necesario, y puedes inspeccionar o compactar el contexto con comandos de barra.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio si aún no está abierto.
2. Escribe `/context`.
3. Observa el modelo, el uso actual de tokens y la distribución del contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio disponible.
4. Compacta la conversación con `/compact`.
5. Escribe `/context` de nuevo y compara los resultados. Es posible que no haya grandes diferencias si la conversación ya es breve.

> [!NOTE]
> Copilot CLI compacta automáticamente el contexto cuando se llena la ventana. Usa `/compact` para elegir cuándo hacerlo. Usa `/clear` o `/new` al cambiar a una tarea no relacionada y quieras iniciar una conversación nueva.

## Elige el modelo

Los modelos tienen distintas ventajas y cada desarrollador puede tener sus preferencias. Copilot CLI permite consultar y seleccionar el modelo.

1. Escribe `/model`.
2. Explora los modelos disponibles y la información de uso.
3. Mantén el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Comparte una sesión

Compartir el trabajo y lo aprendido ayuda a todo el equipo a mejorar el uso de las herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un vínculo para compartir o publicar un gist de GitHub.

1. Escribe `/help` y revisa las opciones de `/share` disponibles en tu versión.
2. Si quieres compartir la sesión, elige el destino adecuado, como `/share file` para exportar un archivo Markdown local.
3. Revisa el contenido exportado antes de enviarlo o publicarlo. Las exportaciones pueden incluir mensajes, respuestas y detalles del proyecto.

Publicar un vínculo o gist es opcional. No publiques contenido del repositorio o de una conversación que el equipo no quiera compartir.

## Opcional: ampliar el trabajo o delegarlo

Has completado el taller principal. Copilot CLI también ofrece comandos para tareas mayores:

- `/fleet` puede dividir subtareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un árbol de trabajo de Git aislado para una tarea distinta.
- `/delegate` puede enviar una tarea al agente de Copilot en la nube, que trabaja de forma asíncrona y puede abrir una PR.

Estos comandos son opcionales, ya que pueden crear árboles de trabajo adicionales o trabajo remoto. Empieza por una tarea nueva y bien definida, y revisa los resultados mediante el flujo habitual. Para profundizar en el trabajo asíncrono con agentes, continúa con el [taller de agentes en la nube][cloud-workshop].

## Resumen y siguientes pasos

Los comandos de barra permiten configurar Copilot CLI, compartir sesiones y consultar qué ocurre en segundo plano. En esta lección:

- has usado `/context` y `/compact` para explorar la gestión del contexto.
- has usado `/model` para explorar los modelos disponibles.
- has aprendido cómo `/share` puede exportar o compartir una sesión.
- has explorado comandos opcionales para el trabajo en paralelo, los árboles de trabajo y la delegación.

Copilot CLI incluye muchos más comandos de barra. Para terminar el recorrido, [repasarás lo aprendido][next-lesson] y descubrirás los siguientes pasos.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca del agente de Copilot en la nube][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
