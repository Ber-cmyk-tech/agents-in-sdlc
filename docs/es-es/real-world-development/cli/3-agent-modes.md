---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear filtros a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos por añadir una pequeña función al proyecto, pero los cambios mayores requieren un proceso más completo. GitHub Copilot CLI está diseñado para trabajar con el flujo de tu organización y ayudarte a crear las funciones adecuadas de la forma correcta. En esta lección y las siguientes seguirás un proceso habitual de desarrollo con agentes: partir de una incidencia, planificar una función, validar el código y el comportamiento, y finalmente fusionar el cambio.

> [!NOTE]
> Seguirás trabajando con la misma conversación y rama durante el flujo de desarrollo de la función. Normalmente usarías distintas ramas o PR para los diferentes tipos de archivo, pero aquí simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación de Copilot nueva a partir de una incidencia de GitHub.
- definirás los requisitos en el modo Plan.
- implementarás la función con el modo Autopilot.
- revisarás el código.
- validarás la función manualmente en un navegador reenviado.

Más adelante actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks`, añadirás validación MCP, crearás un agente de QA y abrirás la PR de la función.

## Escenario

El catálogo de Tailspin Toys está creciendo y los visitantes necesitan filtrar los juegos por categoría y editor. La incidencia del trabajo pendiente describe la función, pero hay detalles —como la combinación de categorías— que deben acordarse antes de programar. Usarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con IA al flujo de desarrollo no cambia los fundamentos. De hecho, los hace aún más importantes. La mayoría de los desarrolladores siguen un proceso parecido a este:

1. Partir de una incidencia que describa el trabajo.
2. Crear un plan de lo que se debe desarrollar.
3. Desarrollar y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Fusionar el código después de que se revise y se supere la integración continua.

> [!NOTE]
> Los detalles concretos dependen del equipo y de la organización, pero la mayoría de los procesos siguen un patrón parecido.

Si sigues este enfoque estándar, te aseguras de que el código generado por IA cumple los requisitos y pasa por el mismo proceso de revisión que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla la autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: trabajas con el agente. Propone cambios y espera tu respuesta antes de continuar.
- **Plan**: el agente primero crea un plan y no puede editar archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en el modo Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Parte de la incidencia

Antes de empezar a trabajar en el filtrado, vuelve a tu Codespace y confirma que el repositorio y el terminal están listos.

1. Vuelve a tu Codespace. Si está detenido, reinícialo.
2. Confirma que la PR de las valoraciones con estrellas está fusionada.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de dirección para resaltar **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al mensaje y volver a la pestaña **Session**.

Observa que el mensaje ahora empieza por `#7` (o un número parecido). El carácter `#` permite incluir en el contexto una incidencia o PR de GitHub.

## Planifica el filtrado

La planificación te permite definir cómo se implementará una función antes de delegar el trabajo a Copilot. Para tareas complejas, dedica tiempo a planificar. Cambia al modo Plan y pide a Copilot que cree el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador debajo del mensaje muestra **Plan**.
2. Después de la referencia a la incidencia que añadiste en el paso anterior, escribe:

   ```plaintext
   Crea un plan para implementar esta función.
   ```

Copilot explora el proyecto y determina el enfoque más adecuado.

3. Copilot puede hacer preguntas sobre el funcionamiento del filtrado. Respóndelas según tus preferencias; no hay una única respuesta correcta.
4. Cuando el plan esté listo, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandirlo.
5. Desplázate por el plan para revisarlo.
6. Pide a Copilot que cambie cualquier parte que no refleje tus decisiones.

## Autoriza Autopilot

Con el plan escrito y revisado, es hora de implementarlo.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente en tu versión.
2. Confirma que el indicador de modo debajo del mensaje muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan, genera código y ejecuta pruebas.

> [!NOTE]
> Al autorizar, la implementación puede empezar inmediatamente, así que revisa primero el plan. Si Copilot informa de dependencias ausentes o de un conflicto de puertos, resuelve el problema de configuración antes de dar por completadas las comprobaciones.

## Revisa y verifica la implementación

Como con cualquier otro código, debes revisar el código generado antes de fusionarlo.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar en el modo Interactive. Confirma que el indicador ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y las decisiones que tomaste al planificar.
4. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que corrija los errores.

## Explora la nueva funcionalidad

Inicia la aplicación y abre el sitio a través del puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Inicia la aplicación para que pueda probar el filtrado en el navegador. Dime la URL y deja el servidor en ejecución.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros de categoría y editor, además de las combinaciones acordadas en el plan.
4. Confirma que el restablecimiento y el comportamiento cuando no hay resultados coinciden con la incidencia y tus decisiones.
5. Vuelve a tu Codespace y pide a Copilot que detenga el servidor de desarrollo que inició.

## Resumen y siguientes pasos

Has usado distintos modos de conversación para crear y revisar una función. En esta lección:

- iniciaste una conversación de Copilot desde una incidencia de GitHub.
- definiste los requisitos en el modo Plan.
- implementaste la función con el modo Autopilot.
- revisaste el código.
- validaste la función manualmente en un navegador reenviado.

A continuación, [explorarás cómo se genera el código y cómo seguir las prácticas documentadas mediante instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
