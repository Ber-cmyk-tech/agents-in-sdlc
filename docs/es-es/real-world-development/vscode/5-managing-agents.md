---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando usas el modo de agente de GitHub Copilot, este trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Copilot Chat en VS Code muestra en tiempo real las llamadas a herramientas y las ediciones. Puedes revisar cada diferencia, aceptar o rechazar cambios y dirigir la conversación con mensajes de seguimiento sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás las ediciones propuestas.
- pedirás al agente que añada un control para el modo claro además del modo de contraste alto.

## Escenario

El agente personalizado de accesibilidad del [Ejercicio 4][previous-lesson] está implementando el modo de contraste alto. Mientras trabaja, el equipo de diseño solicita también un control para el modo claro, para quienes consideran que el tema predeterminado es demasiado oscuro. En vez de iniciar una conversación nueva, ampliarás la tarea desde la sesión actual.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: archivos que explora, comandos que ejecuta y cambios que propone. Cada edición aparece con una diferencia y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat desde el Ejercicio 4.
2. Mientras trabaja el agente, observa sus llamadas a herramientas, lecturas y ediciones de archivos y solicitudes para ejecutar comandos.
3. Cuando proponga una edición, selecciona **Keep** para aceptarla o **Undo** para revertirla y permitir que el agente vuelva a intentarlo.
4. Abre **Files changed** en la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta necesita permiso (por ejemplo, para ejecutar un comando del terminal), el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de contraste alto. El mensaje final resumirá los cambios realizados.

## Pide al agente que añada el modo claro

Amplía la misma conversación para añadir un control de modo claro. El agente conserva el contexto de su trabajo anterior, incluidos los componentes y los patrones de persistencia que haya usado.

1. Envía este mensaje de seguimiento en Copilot Chat:

   ```plaintext
   Buen trabajo. Ahora añade un control del modo claro junto al del contraste alto. El modo claro debe seguir el mismo patrón, persistir en el almacenamiento local y estar disponible en la misma zona de la interfaz. El usuario debe poder activar el contraste alto y el modo claro de forma independiente.
   ```

2. El agente retomará el trabajo, revisará los cambios que ya ha hecho y propondrá las modificaciones necesarias para el modo claro.

3. Revisa cada edición y usa **Keep** o **Undo**.
4. Si el agente toma una dirección que no quieres —por ejemplo, intenta reorganizar los controles—, detenlo con **Stop** y envíale una aclaración.

> [!TIP]
> Dirigir una sesión en curso es más rápido que empezar de nuevo. Usa mensajes de seguimiento para solicitar ajustes (por ejemplo, «simplifica el CSS: usa variables CSS en lugar de duplicar selectores»), corregir el rumbo («no añadas un tercer control; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisa los cambios

Antes de confirmar el trabajo, revisa todos los archivos modificados.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista de cambios. Deberías ver actualizaciones de componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos y revisa las diferencias. Confirma que incluyen atributos ARIA, navegación por teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés satisfecho, prepara y confirma los cambios en el panel Source Control. Los publicarás en la [siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has usado el modo de agente de Copilot Chat para supervisar y dirigir una sesión en curso: revisar ediciones, aceptarlas o rechazarlas y enviar indicaciones de seguimiento para ampliar el trabajo sin perder contexto.

En esta lección has aprendido a supervisar el trabajo del agente en tiempo real, aceptar o rechazar ediciones y dirigir una sesión mediante indicaciones de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad como una solicitud de incorporación de cambios y revisarás los cambios del agente local personalizado.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
