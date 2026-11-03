---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot son asistentes de IA especializados en tareas o ámbitos concretos del flujo de desarrollo. Al definirlos mediante archivos Markdown en `.github/agents`, puedes proporcionar instrucciones, prácticas recomendadas, patrones de programación y conocimiento específico para orientar el trabajo. Los equipos pueden convertir sus conocimientos en agentes reutilizables, como uno de accesibilidad que compruebe el cumplimiento de [WCAG][wcag], uno de seguridad o uno de pruebas.

Los agentes se definen en archivos Markdown en `.github/agents` o globalmente en `~/.copilot/agents`. Cada archivo tiene frontmatter YAML que contiene, como mínimo, `name` y `description`, seguido de un mensaje que define el comportamiento, la experiencia y las instrucciones del agente.

### Diferencias entre agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] se definen principalmente mediante archivos Markdown, pero cumplen funciones distintas: un **agente personalizado** es el trabajador y las **habilidades** son herramientas. Un agente personalizado tiene su propia ventana de contexto y puede coordinar habilidades e incluso otros agentes. En este laboratorio, el agente de accesibilidad revisará y actualizará el sitio siguiendo directrices de accesibilidad; para ello, podría usar habilidades de flujo de PR o de ejecución de pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Como con cualquier herramienta de IA, prueba distintas opciones e itera para encontrar las que mejor se adapten a tu entorno y tus escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todas las personas. Algunos usuarios han indicado que el tema oscuro actual es difícil de leer por el bajo contraste entre el texto y el fondo. El equipo de diseño solicita un modo de contraste alto que se pueda activar y desactivar.

La accesibilidad es importante y quieres implementar la función rápidamente. Usarás un agente personalizado para generar la funcionalidad.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad.
- usarás el agente de accesibilidad en Copilot Chat para implementar un modo de contraste alto.

## Revisa el agente personalizado de accesibilidad

El proyecto ya incluye un agente de accesibilidad. Revísalo para saber cómo orientará a Copilot.

Vuelve a tu codespace y crea una rama nueva a partir de `main` para mantener separada la tarea de accesibilidad de la PR de filtrado del Ejercicio 3:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Revisa los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

3. Revisa las responsabilidades principales, las prácticas recomendadas de accesibilidad, los ejemplos de HTML, CSS y JavaScript, y los errores frecuentes.

## Usa el agente personalizado en Copilot Chat

VS Code muestra los agentes personalizados de `.github/agents` en el selector de agentes de Copilot Chat. Puedes seleccionar uno para aplicar sus instrucciones y herramientas a la sesión.

> [!TIP]
> **Abre Copilot Chat**
>
> Vuelve al codespace, abre Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva.

1. En la vista Chat, selecciona **Agent** si aún no está seleccionado.

   ![Captura del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona el menú de agentes situado en la parte inferior de la vista Chat (muestra el agente activo; de forma predeterminada es **default**).
3. En la lista de agentes disponibles, selecciona **Accessibility agent**.
4. Envía este mensaje al agente de accesibilidad:

   ```plaintext
   Añade al sitio un modo de contraste alto. Debe haber un control que el usuario pueda activar para habilitar el contraste alto, y la selección debe persistir entre recargas mediante el almacenamiento local del navegador.
   ```

5. Copilot Chat explorará el código, propondrá cambios y aplicará ediciones a los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y una diferencia que podrás revisar.
6. A medida que se apliquen las ediciones, se actualizará el indicador **Files changed** con los archivos que haya modificado el agente.

> [!NOTE]
> El proceso puede tardar varios minutos. Copilot modificará archivos reales del repositorio, como componentes Astro, CSS y las pruebas relacionadas.

Revisarás y dirigirás el trabajo en curso en el siguiente ejercicio.

## Resumen y siguientes pasos

Has explorado [agentes personalizados][custom-agents] de GitHub Copilot, asistentes especializados en tareas y ámbitos concretos. Permiten convertir la experiencia y las normas de un equipo en agentes reutilizables.

En esta lección has aprendido cómo se definen los agentes personalizados y cómo usarlos en el modo de agente de Copilot Chat.

A continuación, [supervisarás y dirigirás el trabajo del agente][next-lesson], revisarás los cambios mientras se generan y añadirás un control para el modo claro en la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Probar la función con el servidor Playwright MCP][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
