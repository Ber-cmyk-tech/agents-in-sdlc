---
title: "Ejercicio 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot son asistentes de IA especializados en tareas o ámbitos concretos del desarrollo. Puedes definirlos mediante archivos Markdown en `.github/agents`, donde se especifican instrucciones, prácticas recomendadas y conocimiento específico. Los equipos pueden convertir su experiencia en agentes reutilizables, como uno de accesibilidad que compruebe el cumplimiento de [WCAG][wcag], uno de seguridad o uno de pruebas.

Los agentes se definen en archivos de `.github/agents` o globalmente en `~/.copilot/agents`. Cada archivo tiene frontmatter YAML con, al menos, `name` y `description`, seguido de las instrucciones y el comportamiento del agente.

### Diferencias entre agentes personalizados y habilidades de agente

Tanto los agentes como las [habilidades][agent-skills-concept] se definen principalmente mediante archivos Markdown. La diferencia más sencilla es que el **agente personalizado** es el trabajador y las **habilidades** son herramientas. Cada agente tiene su propia ventana de contexto y puede coordinar habilidades y otros agentes. En este laboratorio, el agente de accesibilidad revisará el sitio y puede usar habilidades para gestionar solicitudes de incorporación de cambios o pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Prueba e itera para encontrar lo que funciona en tu entorno y tus escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys quiere que su plataforma sea accesible para todas las personas. Algunos usuarios tienen dificultades para leer el tema oscuro por el bajo contraste entre el texto y el fondo. El equipo de diseño solicita implementar un modo de contraste alto que se pueda activar y desactivar. Usarás un agente personalizado para desarrollar la función con rapidez.

En este ejercicio explorarás cómo se definen los agentes personalizados y cómo se les asignan tareas.

## Revisa el agente personalizado de accesibilidad

El proyecto ya incluye un agente personalizado de accesibilidad. Revisa su contenido para entender cómo guiará a Copilot. Vuelve al codespace y abre `.github/agents/accessibility.md`.

1. Lee los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> El frontmatter con los campos `name` y `description` es obligatorio para los agentes personalizados.

2. Desde ahí, revisa las secciones que describen:
   - las responsabilidades principales al generar código para un sitio web accesible.
   - las prácticas recomendadas de accesibilidad.
   - ejemplos de código HTML, CSS y JavaScript.
   - errores y problemas habituales.

## Crea y asigna una incidencia

El centro de control es el lugar principal para trabajar con agentes en tu entorno. Puedes asignar tareas al agente de Copilot en la nube, supervisarlas y redirigir o guiar su trabajo. Empecemos asignándole la tarea de implementar el modo de contraste alto.

1. Ve a tu repositorio.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de creación.
4. Selecciona **Blank issue**.
5. Escribe `Add high contrast mode to website` en **Title**.
6. Escribe lo siguiente en **Description**:

   ```plaintext
   Necesitamos un modo de contraste alto para el sitio. Debe haber un control que el usuario pueda activar. La preferencia debe guardarse en el almacenamiento local del navegador.
   ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de asignación.
9. En la lista, selecciona **Accessibility agent**.

   ![Captura del cuadro de asignación del agente en la nube, con los campos de agente personalizado y de accesibilidad resaltados](../../../_images/ex5-select-custom-agent.png)

10. Selecciona **Assign**.
11. Copilot empezará a trabajar en la tarea en segundo plano.

## Resumen y siguientes pasos

Has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes especializados en tareas y ámbitos concretos. Permiten convertir la experiencia y los estándares del equipo en roles reutilizables que orientan el trabajo de Copilot.

Has aprendido cómo se definen los agentes personalizados y cómo se asigna una tarea a uno de ellos. Ahora que Copilot está trabajando en el modo de contraste alto, puedes [supervisar y dirigir la sesión del agente][next-lesson] desde el centro de control.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Preparación de agentes personalizados en una organización][org-custom-agents]
- [Preparación de agentes personalizados en una empresa][enterprise-custom-agents]

---

| [← Lección anterior: Agente de GitHub Copilot en la nube][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
