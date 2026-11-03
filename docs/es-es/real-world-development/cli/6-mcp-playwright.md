---
title: "Lección 6 - Validar la funcionalidad con Playwright MCP"
description: "Inspecciona o configura Playwright MCP en Copilot CLI y úsalo para explorar en el navegador la función de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Escribir código no es lo único que hace falta: también trabajamos con datos y servicios externos, y podemos ofrecer a Copilot más automatizaciones. Los servidores MCP permiten que Copilot use herramientas y servicios que no están integrados en la CLI.

En esta lección:

- aprenderás qué es el Protocolo de contexto de modelo (MCP) y cómo lo usa Copilot CLI.
- añadirás el servidor Playwright MCP si aún no está disponible.
- pedirás al agente que controle un navegador y explore la función de filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero para validar cambios en la interfaz de usuario también hay que interactuar con ella. Permitirás que Copilot use el sitio web como un usuario para automatizar aún más la validación de los cambios.

## ¿Qué es el Protocolo de contexto de modelo (MCP)?

El [Protocolo de contexto de modelo (MCP)][mcp-blog-post] permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real, accedan a información actualizada y realicen acciones en tu nombre. Un servidor MCP actúa como puente entre el agente y las herramientas y recursos externos.

Algunos servidores MCP populares son:

- **[GitHub MCP Server][github-mcp]**: da acceso a API para gestionar repositorios, incidencias y solicitudes de incorporación de cambios de GitHub.
- **[Playwright MCP Server][playwright-mcp-server]**: ofrece automatización del navegador mediante Playwright.

Hay muchos más servidores MCP. GitHub mantiene un [registro MCP][mcp-registry] para facilitar su descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, verifica quién lo publica y considera las implicaciones de seguridad.

## Añade el servidor Playwright MCP

Si el servidor Playwright MCP aún no está disponible, añádelo para que Copilot pueda interactuar con el sitio como lo haría un usuario.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos y continúa con la sección siguiente.

1. Vuelve a tu Codespace.
2. En Copilot CLI, ejecuta:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Confirma el tipo de servidor **STDIO** con <kbd>Enter</kbd> y pulsa <kbd>Tab</kbd>.
5. En el cuadro **Command**, escribe:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de MCP.

## Pide a Copilot que explore la función con Playwright

Usa el servidor Playwright MCP de Copilot para comprobar el comportamiento:

1. Envía este mensaje:

   ```plaintext
   Inicia la aplicación y usa Playwright MCP para comprobar el filtrado según la incidencia y el plan acordado. Dime qué funciona y qué no, sin hacer cambios. Cuando termines, detén el servidor que has iniciado.
   ```

> [!NOTE]
> No es imprescindible decirle a Copilot que use el servidor MCP; normalmente lo averiguará por su cuenta. Aun así, indicar la herramienta que debe usar puede hacer que los resultados sean más uniformes y ahorrar tokens.

2. Observa los pasos que Copilot realiza en el navegador.
3. Lee el informe y comprueba que todo funciona según lo previsto.

Copilot iniciará el servidor, interactuará con el sitio mediante Playwright, detendrá el servidor y te entregará un informe.

## Resumen y siguientes pasos

¡Enhorabuena! Has usado Playwright MCP para explorar tu función en un navegador real desde Copilot CLI. En resumen:

- has aprendido qué es MCP y cómo lo usa Copilot CLI.
- has añadido el servidor Playwright MCP si aún no estaba disponible.
- has pedido al agente que controle un navegador y explore el filtrado.

A continuación, [crearás un agente personalizado de QA][next-lesson] que combinará la habilidad y las herramientas del navegador en un rol especializado.

## Recursos

- [¿Qué es MCP y por qué habla todo el mundo de ello?][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
