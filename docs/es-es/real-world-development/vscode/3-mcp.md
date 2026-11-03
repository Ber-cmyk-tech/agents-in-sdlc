---
title: "Ejercicio 3 - Probar la función con el servidor Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo de agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear la función de filtrado con el modo de agente. Antes de abrir una solicitud de incorporación de cambios, confirma que funciona en el navegador. Conectarás el **servidor Playwright MCP** para que Copilot controle un navegador real y pruebe la función. Después publicarás la rama y abrirás la PR.

En este ejercicio:

- aprenderás qué es el Protocolo de contexto de modelo (MCP) y cómo los servidores MCP amplían Copilot con nuevas herramientas.
- añadirás el servidor Playwright MCP al espacio de trabajo.
- pedirás a Copilot que pruebe manualmente el filtrado en un navegador.
- publicarás la rama y abrirás una solicitud de incorporación de cambios.

## ¿Qué es el Protocolo de contexto de modelo (MCP)?

El modo de agente tiene muchas más capacidades cuando puede acceder a recursos externos. El Protocolo de contexto de modelo (MCP) permite a Copilot comunicarse con herramientas y servicios externos mediante un estándar común.

![Diagrama del funcionamiento interno del modo de agente y de cómo interactúa con el contexto, el LLM y las herramientas, incluidas las herramientas que proporcionan los servidores MCP y las extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

El [Protocolo de contexto de modelo (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) proporciona a los agentes de IA una forma de comunicarse con herramientas y servicios externos. Así pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

El agente accede a herramientas y recursos a través de un servidor MCP, que actúa como puente con los servicios externos. El servidor gestiona la comunicación entre el agente y las herramientas externas, como API existentes o herramientas locales (por ejemplo, paquetes de NPM). Cada servidor MCP representa un conjunto distinto de herramientas y recursos disponibles para el agente.

Algunos servidores MCP populares son:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona un conjunto de API para gestionar repositorios de GitHub. El agente puede crear repositorios, actualizar los existentes y gestionar incidencias y solicitudes de incorporación de cambios.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: ofrece automatización del navegador mediante Playwright. El agente puede navegar por páginas, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP que dan acceso a distintas herramientas y recursos. GitHub mantiene un [registro MCP](https://github.com/mcp) para facilitar el descubrimiento de servidores y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia. Antes de usar uno, revisa su código fuente, comprueba quién lo publica y considera las implicaciones de seguridad. Usa únicamente servidores de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisa la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo.

1. Abre `.vscode/mcp.json` en tu codespace.
2. Comprueba que incluye un servidor `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

La entrada `github` viene incluida en la plantilla del proyecto; por eso Copilot pudo leer el trabajo pendiente de incidencias en el ejercicio anterior. Usa el [servidor GitHub MCP remoto][remote-github-mcp-server], así que no hace falta instalarlo localmente: VS Code se conecta por HTTP y te pedirá que autentiques tu cuenta de GitHub la primera vez que Copilot use una de sus herramientas.

## Añade el servidor Playwright MCP

Ahora añadirás un segundo servidor. El [servidor Playwright MCP][playwright-mcp-server] proporciona a Copilot un navegador controlable para probar la función.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Guarda el archivo.

El servidor `playwright` es local: VS Code lo inicia mediante `npx @playwright/mcp@latest`. La opción `--headless` ejecuta el navegador sin ventana, algo necesario en un codespace sin escritorio.

> [!NOTE]
> Tailspin Toys ya utiliza Playwright para las pruebas de extremo a extremo, por lo que normalmente el navegador ya está instalado. Si Copilot informa de que falta, pídele que ejecute `npx playwright install chromium`.

## Inicia y autoriza el servidor

VS Code inicia los servidores MCP cuando se necesitan por primera vez y te pide que confirmes si confías en ellos.

1. En `.vscode/mcp.json`, selecciona **Start** sobre la entrada `playwright` para iniciar el servidor y verificar su configuración.
2. Si VS Code solicita confirmación, revisa la configuración y selecciona la opción para confiar en el servidor.

Este paso es opcional; si no lo inicias manualmente, VS Code lo hará cuando Copilot necesite sus herramientas.

## Prueba la función de filtrado

El [servidor Playwright MCP][playwright-mcp-server] permite a Copilot controlar un navegador real. En vez de que tú recorras el sitio para comprobar el trabajo, el agente puede abrir una página, navegar, aplicar filtros y leer los resultados; después te resume lo que ha observado. Así puedes confirmar rápidamente que la función se comporta como esperas sin salir de la conversación.

Internamente, Playwright MCP usa el [árbol de accesibilidad][playwright-mcp-server] de la página en lugar de capturas de pantalla. El agente interpreta elementos estructurados y etiquetados, como botones, vínculos y elementos de lista, igual que las tecnologías de asistencia. Por eso, una comprobación funcional rápida también ofrece una validación básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, envía este mensaje:

```text
Usa el servidor Playwright MCP para abrir un navegador en la aplicación que se está ejecutando en http://localhost:4321 y comprobar la nueva función de filtrado de juegos:

1. Ve a la página de juegos y anota cuántos juegos aparecen.
2. Aplica un filtro de categoría y confirma que la lista solo muestra juegos de esa categoría.
3. Elimínalo, aplica un filtro de editor y confirma que la lista solo muestra los juegos de ese editor.
4. Combina un filtro de categoría y otro de editor, y confirma que los resultados respetan ambos.

Informa de lo que observes en cada paso y señala cualquier comportamiento inesperado.
```

Copilot abrirá un navegador mediante Playwright MCP, seguirá los pasos y te informará de los resultados. Compara el informe con los criterios de aceptación de la incidencia y pide cambios si algo no funciona.

> [!NOTE]
> Para esta prueba, la aplicación debe estar en ejecución en `http://localhost:4321`. Si has detenido el servidor, vuelve a iniciarlo. La primera vez que se use Playwright MCP puede ser necesario descargar el navegador; si falta, ejecuta `npx playwright install chromium`.

## Publica la rama y crea una solicitud de incorporación de cambios

1. Abre el panel **Source Control** del codespace y revisa los cambios de Copilot.
2. Prepara los cambios con el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.
4. Selecciona **Publish** para enviar la rama al repositorio.

![Captura del panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

Pide a Copilot que cree una PR y la vincule con la incidencia correspondiente:

1. En Copilot Chat, selecciona **New Chat**.
2. Selecciona **Agent** para que Copilot pueda usar las herramientas de GitHub.
3. Envía este mensaje:

   ```plaintext
   Busca en el repositorio la incidencia relacionada con filtrar por categoría y editor. Crea una solicitud de incorporación de cambios para la rama actual y vincúlala con la incidencia correcta.
   ```

4. Cuando sea necesario, selecciona **Continue** para permitir que Copilot recopile información y realice las operaciones. La primera vez que use una herramienta de GitHub, puede pedirte que autentiques tu cuenta; sigue las indicaciones.
5. Revisa la PR mediante el vínculo de Copilot, pero **no la fusiones todavía**.

## Resumen y siguientes pasos

En este ejercicio:

- has aprendido qué es MCP y cómo amplía Copilot con nuevas herramientas.
- has añadido Playwright MCP al espacio de trabajo.
- lo has usado para probar el filtrado manualmente en el navegador.
- has publicado la rama y abierto una PR.

Has usado un servidor MCP para probar una función. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas específicas, como las revisiones de accesibilidad.

## Recursos

- [¿Qué es MCP y por qué habla todo el mundo de ello?][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [Servidor GitHub MCP][github-mcp-server]
- [Registro GitHub MCP][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
