---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente de Copilot en la nube que realice tres tareas para mejorar la experiencia y añadir funciones. Aunque trabaja de forma asíncrona y autónoma, es importante supervisar las tareas.

Puedes gestionar las tareas asignadas al agente desde la [página de agentes][agents-page] de GitHub.com. Allí puedes ver las tareas que tienen PR abiertas, explorar las operaciones realizadas y dirigir una sesión que esté en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas.
- dirigirás una sesión en curso para solicitar otra función.

## Escenario

Después de pedir al agente que añadiera un modo de contraste alto, el equipo decide añadir también un modo claro. Como ya se están actualizando los estilos y los controles, pedirás al agente que añada el modo claro en la misma tarea.

## Revisa las tareas del agente en la nube

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Revisa las tareas de los paneles principal e izquierdo, como la documentación del código, las API para modificar juegos y el modo de contraste alto.
3. Selecciona una tarea activa y revisa las operaciones realizadas por Copilot, como descargar el código, preparar el entorno, configurar servidores MCP y completar la tarea.
4. Revisa el panel de PR, que muestra la solicitud y los archivos modificados.

> [!NOTE]
> Los pasos concretos que aparecen dependen del estado de la tarea y del enfoque que siga Copilot.

## Dirige el agente en la nube

Pide a Copilot que añada el modo claro mientras trabaja en el modo de contraste alto.

1. Selecciona la sesión relacionada con el modo de contraste alto.

    ![Sesión de accesibilidad en el centro de control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado de configurar el entorno y ha empezado a trabajar. Lo sabrás cuando empiecen a aparecer mensajes como los siguientes.
3. En el cuadro **Steer active session while Copilot is working**, escribe:

   ```plaintext
   Mientras trabajamos en el modo de contraste alto, añadamos también un modo claro. Debe haber otro control para que los usuarios puedan elegir el modo de visualización que prefieran.
   ```

    ![Tarea del agente en la nube en la página de agentes, con el cuadro para dirigir la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el mensaje.
5. Comprueba que Copilot confirma la indicación y la incorpora al trabajo.

Copilot incorporará la nueva solicitud cuando termine el paso en curso. El proceso puede tardar varios minutos; puedes aprovechar para repasar lo aprendido.

## Resumen y siguientes pasos

Has explorado la página de agentes de Copilot, el centro de control para supervisar y dirigir las tareas del agente en la nube. En esta lección has supervisado tareas y dirigido una sesión activa para solicitar otra función.

A continuación, [iterarás sobre las solicitudes de incorporación de cambios de Copilot][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
