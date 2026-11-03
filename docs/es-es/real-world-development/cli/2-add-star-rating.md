---
title: "Lección 2 - Añadir valoraciones con estrellas: una mejora rápida"
description: "Usa Copilot CLI para modificar las tarjetas de juegos, revisar el cambio en un navegador reenviado y fusionarlo en tu primera solicitud de incorporación de cambios."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ahora que has instalado Copilot CLI y probado una conversación, harás tu primer cambio en el proyecto. Será pequeño: los datos de los juegos ya incluyen una valoración con estrellas, pero las tarjetas de la página principal aún no la muestran. Pedirás al agente que la muestre, revisarás el cambio y lo fusionarás en tu primera solicitud de incorporación de cambios.

En esta lección:

- iniciarás una conversación de Copilot centrada en una rama de funcionalidad.
- pedirás al agente un pequeño cambio en el proyecto.
- revisarás el cambio con `/diff`.
- ejecutarás la aplicación y comprobarás el cambio en un navegador reenviado.
- abrirás y fusionarás tu primera solicitud de incorporación de cambios.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración con estrellas, que ya aparece en la página de detalles del juego. Sin embargo, las tarjetas de la página principal solo muestran el título, la categoría, el editor y la descripción. Para empezar, pedirás al agente que muestre la valoración existente en cada tarjeta: un cambio pequeño y autónomo, ideal para tu primera sesión.

## Estructura de una conversación

Una **conversación** es el espacio donde trabajas con Copilot CLI en una tarea. A diferencia de la aplicación Copilot, una conversación normal de CLI usa el repositorio y la rama que están abiertos en el terminal, en vez de crear un árbol de trabajo dedicado. Las conversaciones guardadas te permiten volver a la misma conversación; los archivos y la rama siguen siendo el estado habitual de Git en el disco.

En una conversación verás tus mensajes y las respuestas del agente, su actividad con herramientas mientras explora y edita archivos, y los cambios que puedes inspeccionar con `/diff`.

## Inicia una conversación y solicita el cambio

1. Vuelve a tu Codespace.
2. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Si Copilot no está iniciado, ejecuta:

   ```bash
   copilot --yolo
   ```

4. Inicia una nueva sesión con el comando de barra `/new` y pulsa <kbd>Enter</kbd>.
5. Envía este mensaje:

   ```plaintext
   Muestra el valor de starRating sobre 5 en cada tarjeta de juego de la página de lista. Si la valoración es null, muestra «Aún no hay valoraciones». Mantén el diseño de las tarjetas, añade pruebas y ejecuta las comprobaciones pertinentes.
   ```

Copilot explora el proyecto, localiza los archivos que muestran los detalles de los juegos y crea el código necesario. ¡Ya has añadido una función con Copilot CLI!

## Revisa la diferencia

Antes de fusionar cualquier cambio generado por IA, aunque sea pequeño, debes revisarlo.

1. Escribe `/diff` e inspecciona todos los archivos modificados.
2. Confirma que la tarjeta muestra la valoración numérica cuando existe y «Aún no hay valoraciones» cuando `starRating` es `null`.
3. Confirma que las pruebas cubren ambos casos.
4. Revisa los resultados de las comprobaciones que ejecutó Copilot y pídele que corrija los errores.
5. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.

> [!NOTE]
> Copilot, como todas las herramientas de IA generativa, es probabilístico, no determinista; por eso, el código concreto puede variar. Revisa el comportamiento en vez de esperar una implementación exacta.

## Comprueba los cambios

No basta con leer el código y dar por hecho que funciona. Pide a Copilot que inicie el sitio para que puedas revisar la interfaz de usuario (UI) en el navegador reenviado por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Inicia la aplicación para que pueda revisar en el navegador el cambio de las valoraciones con estrellas. Dime la URL y deja el servidor en ejecución.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Confirma que las tarjetas muestran sus valoraciones sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor que inició:

   ```plaintext
   Detén el servidor de desarrollo que has iniciado.
   ```

## Abre y fusiona tu primera solicitud de incorporación de cambios

Ya has creado la función. Ahora crearás una solicitud de incorporación de cambios (PR) para fusionar el código en el proyecto.

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Confirma los cambios revisados de las valoraciones con estrellas con un mensaje de confirmación adecuado.
   ```

2. Escribe `/pr create`. Copilot CLI publicará la confirmación existente al crear la PR y mostrará su URL.
3. Abre la PR manteniendo pulsada <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la URL que muestra Copilot CLI.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando esté todo listo, selecciona **Merge pull request** y confirma la fusión.
6. Vuelve a tu Codespace y sal de Copilot CLI con `/exit`.
7. Actualiza tu `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y siguientes pasos

¡Enhorabuena! Has publicado tu primer cambio con GitHub Copilot CLI. En concreto:

- iniciaste una conversación de Copilot centrada en una rama de funcionalidad.
- dirigiste al agente para que modificara las tarjetas de juegos.
- revisaste el cambio con `/diff`.
- ejecutaste la aplicación y comprobaste la valoración en un navegador reenviado.
- abriste y fusionaste tu primera solicitud de incorporación de cambios.

A continuación, [partirás de la incidencia de filtrado y usarás los modos Plan y Autopilot][next-lesson] para desarrollar una función más amplia.

## Recursos

- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
