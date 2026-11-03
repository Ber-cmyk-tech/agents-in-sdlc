---
title: "Ejercicio 2 - Añadir funciones con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso los cambios más sencillos de una aplicación suelen requerir modificaciones en varios archivos y operaciones, como ejecutar pruebas. En el flujo de trabajo de un desarrollador, esto suele implicar localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo se ha pasado por alto y volver a modificarlo. Y así sucesivamente.

Para eso sirve el modo de agente de Copilot.

El modo de agente de Copilot está diseñado para trabajar con más autonomía en el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, realiza las modificaciones necesarias, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. En este ejercicio usarás el modo de agente para añadir funciones al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en tu IDE para permitir ciclos de respuesta rápidos e interacción. El agente en la nube trabaja de forma asíncrona como un miembro del equipo y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- dejar que el modo de agente de Copilot explore el proyecto, identifique los archivos pertinentes y coordine los cambios.
- implementar nuevas funciones en la interfaz de usuario (UI) y la capa de datos.
- revisar los cambios y las pruebas generados por el modo de agente antes de fusionarlos en el código.

## Escenario

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editor. En el ejercicio anterior añadiste una función auxiliar de editores; ahora completarás el resto del trabajo de la capa de datos, la interfaz y las pruebas con el modo de agente de Copilot.

## Ejecuta el sitio web Tailspin Toys

Antes de hacer cambios, explora el sitio web Tailspin Toys para conocer su funcionamiento actual.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar juegos y sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM en el frontmatter; no hay una API de backend independiente ni un framework de interfaz de cliente. Las funciones auxiliares reutilizables de acceso a datos se encuentran en `src/lib/`, y la interactividad se implementa con un elemento `<script>` de Astro de ámbito reducido y las API estándar del DOM.

### Inicia el sitio web

Para facilitar la ejecución del sitio, se incluye la tarea `npm run dev`, que inicia el único servidor de desarrollo de Astro. Puedes ejecutarla en GitHub Codespaces:

1. Vuelve a tu codespace. Después del ejercicio anterior deberías estar en `main`.
2. Abre un terminal en el codespace con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea y cambia a una rama nueva para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo Astro esté listo, verás un mensaje con la URL, similar a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir `http://localhost:4321` en el navegador, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando en el terminal la dirección del servidor Astro `http://localhost:4321`.

> [!NOTE]
> En un codespace, al seleccionar el vínculo localhost del terminal se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado que permite acceder al servidor web que se ejecuta en el codespace.

### Explora el sitio web

Cuando se esté ejecutando, podrás explorar las funciones principales:

- **Home Page**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Game Details Page**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, el editor y la categoría.

## Explora el trabajo pendiente con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve a tu codespace, abre Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo dependen del ejercicio; se indicarán cuando corresponda.

La implementación inicial del sitio funciona, pero queremos añadir nuevas capacidades. Primero revisaremos el trabajo pendiente. Al crear el proyecto a partir de la plantilla, se generaron automáticamente incidencias; pide a GitHub Copilot que te las muestre.

1. En el selector de agentes de la vista Chat, selecciona **Agent**. El agente planifica e implementa cambios en varios archivos, ejecuta comandos del terminal e invoca herramientas de forma autónoma.

   ![Captura del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. En la lista de modelos disponibles, selecciona **Claude Sonnet 4.5**.

> [!CAUTION]
> Los autores del taller no prefieren un modelo sobre otro. Al crear este taller usamos Claude Sonnet 4.5 y lo incluimos en las instrucciones para que las sugerencias de código sean relativamente coherentes y la experiencia sea más uniforme. Como los modelos de lenguaje son probabilísticos, es posible que recibas sugerencias distintas a las que se describen aquí. Es normal y esperable.

> [!NOTE]
> Debido a la naturaleza probabilística de los modelos de lenguaje, Copilot puede utilizar un comando MCP diferente y aun así completar la tarea.

3. Envía a Copilot este mensaje para preguntarle por las incidencias pendientes:

   ```plaintext
   Muéstrame el trabajo pendiente de mi repositorio de GitHub. Ayúdame a priorizarlo según las tareas que más beneficiarán a los usuarios.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado los elementos según lo que considera más útil para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto. Usarás el archivo de instrucciones de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio.

1. En el codespace, ve a `.github/instructions/ui.instructions.md`.
2. Revisa las directrices para añadir funcionalidad, que incluyen:
   - una descripción general de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - vínculos a instrucciones específicas para distintos tipos de archivo, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así se pueden reutilizar las instrucciones, dividir los conjuntos grandes en otros más pequeños y ofrecer ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado hay que hacer al menos tres cambios distintos en la aplicación:

- añadir o ajustar la lógica restante de filtrado en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento del filtrado.
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, hay que ejecutar las pruebas y confirmar que se superan antes de fusionar los cambios. El modo de agente de Copilot puede realizar estas tareas. Añade la funcionalidad:

1. Continúa la conversación actual con Copilot o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de cómo seleccionar el archivo de instrucciones de UI](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el selector de agentes de la vista Chat.

   ![Captura del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada del trabajo pendiente. Envía este mensaje:

   ```plaintext
   Actualiza el sitio para incluir filtros por editor y categoría según los requisitos de la incidencia de GitHub relacionada. Ya existe una función auxiliar de editores del ejercicio anterior; consérvala y mejórala si hace falta al completar la capa de datos, la interfaz y las pruebas restantes. Asegúrate de que todas las pruebas se superan antes de terminar. El servidor ya está en ejecución; no hace falta volver a iniciarlo.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos pertinentes. Deberías verlo identificar las funciones auxiliares de la capa de datos, la interfaz y las pruebas, y después modificar archivos y ejecutar las pruebas.

   ![Captura de Copilot mientras explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. Según la complejidad de la tarea y del código, puede tardar varios minutos. Mientras trabaja, es posible que aparezcan los botones **Keep** y **Undo** en el editor. Al terminar, habrá un botón **Keep** o **Undo** para cada cambio. No es necesario seleccionarlos mientras el agente sigue trabajando.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de un diálogo de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que Copilot se detenga varias veces e incluso que algunas pruebas fallen durante el proceso. No pasa nada: alterna entre generar código y ejecutar pruebas hasta completar la tarea sin detectar errores.

   ![Captura de una sesión completada en el modo de agente de Copilot Chat](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has aprendido a usar el modo de agente de GitHub Copilot para añadir funciones a Tailspin Toys. Has aprendido que:

- el modo de agente de GitHub Copilot puede implementar funciones en la interfaz y la capa de datos.
- puede explorar el proyecto, identificar los archivos pertinentes y coordinar cambios.
- debes revisar los cambios y las pruebas que genera antes de fusionar el código.

A continuación, [probarás la función con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar paginación

A medida que crezca la lista de juegos, será necesario habilitar la paginación. Usa lo que has aprendido para pedir a Copilot que la implemente. Ten en cuenta lo siguiente:

- sigue las prácticas recomendadas, incluidas las instrucciones existentes.
- decide cómo quieres implementar la paginación y si permitirás que el usuario elija el tamaño de página o lo fijarás en el código.
- al crear el mensaje, proporciona a Copilot contexto suficiente para implementar la solución que quieres.
- puede que tengas que iterar con Copilot, pedir cambios y proporcionar contexto. Es el proceso normal al trabajar con Copilot.

## Recursos

- [Copilot: modos Ask, Edit y Agent, y cuándo usarlos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
