---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan deuda técnica. Puede tratarse de incidencias de seguridad sin resolver, código heredado que requiere actualizaciones o solicitudes de funciones que llevan tiempo en el trabajo pendiente porque no ha habido tiempo para implementarlas. El agente de GitHub Copilot en la nube está diseñado para tareas como actualizar código y añadir funciones de forma autónoma. Cuando termina, crea un borrador de solicitud de incorporación de cambios (PR) para que un desarrollador lo revise. Así puedes delegar tareas tediosas, acelerar el desarrollo y centrarte en aspectos de mayor impacto.

En esta lección explorarás cómo personalizar el entorno, realizar operaciones de forma segura, definir incidencias con un alcance claro y asignarlas a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica. Los contratistas que crearon la primera versión del sitio dejaron la documentación en un estado deficiente: no hay ninguna. Como primer paso, el equipo quiere comentarios TSDoc en todas las funciones exportadas.

Además, el equipo de diseño quiere mejorar el descubrimiento de juegos. Quiere que la página de detalles de cada juego muestre otros títulos de la misma categoría para que la gente pueda seguir explorando. Por ahora no necesita una implementación pulida: solo una versión que permita probar y aceptar la experiencia de usuario (UX). Esta tarea es un impedimento, pero hay otras incidencias de mayor prioridad.

Son tareas que suelen aplazarse y que se pueden asignar al agente en la nube. Mientras trabaja en segundo plano, puedes continuar con otras tareas y revisar después los cambios.

## Presentación del agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a un desarrollador. Al igual que cuando trabajas con una persona, puedes asignarle tareas de varias formas; por ejemplo, [asignándole una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Una vez asignada, Copilot crea un borrador de PR para seguir el progreso, configura un entorno y empieza a trabajar. Puedes consultar la sesión mientras está en curso o una vez terminada. Cuando la solución propuesta esté lista para revisión, Copilot te mencionará en la solicitud de incorporación de cambios.

## La importancia de definir bien el alcance

GitHub Copilot no tiene poderes mágicos: no elimina la necesidad de pensar. Incluso las tareas aparentemente sencillas pueden ser complejas cuando se examinan de cerca, así que [define cuidadosamente el alcance de las tareas del agente en la nube][cloud-agent-best-practices]. Trátalo como un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian con la IA generativa.

## Instrucciones personalizadas de este repositorio

Los ejercicios anteriores presentaron las instrucciones personalizadas y cómo guían a Copilot. Si ya completaste el ejercicio práctico de instrucciones personalizadas, habrás visto `.github/copilot-instructions.md`; si no, este es un buen momento para revisarlo. Antes de asignar trabajo al agente en la nube, revisa en modo de solo lectura los archivos de instrucciones que ya incluye el repositorio para que después puedas reconocer su efecto.

Abre estos archivos en la interfaz web de GitHub o en un codespace si ya tienes uno:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, sobre todo las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el frontmatter `applyTo`, que aplica estas instrucciones a archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation` en la sección siguiente, comprueba si la PR resultante incluye comentarios TSDoc, convenciones de TypeScript y encabezados. Estas indicaciones proceden de los archivos de instrucciones. Volveremos a señalarlo al revisar las PR en un ejercicio posterior.

## Configura el entorno de desarrollo del agente

Crear código normalmente requiere un entorno específico y ejecutar algunos scripts de configuración para confirmar que todo está listo. Esto también se aplica al agente de Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente en la nube usa [GitHub Actions][github-actions] para preparar el entorno. Puedes personalizarlo con un [flujo de trabajo de configuración][setup-workflow] especial, definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece a trabajar. Esto permite instalar las herramientas y dependencias necesarias. El flujo ya está configurado para este laboratorio: instala Node.js y las dependencias del proyecto, los binarios del navegador para las pruebas de extremo a extremo y configura la base de datos SQLite migrada y precargada para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Usa los permisos mínimos necesarios para los pasos definidos. Copilot recibe su propio token para operar.
    permissions:
      # Si los pasos de configuración clonan el repositorio, por ejemplo para instalar dependencias, necesitarán el permiso contents: read. Si no se clona, Copilot lo hará automáticamente al terminar la configuración.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de la aplicación: Node.js (ahora toda la aplicación usa Astro y Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga la base de datos SQLite local para que la compilación y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`, que GitHub Actions ejecuta antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- El desencadenador `workflow_dispatch` permite ejecutar manualmente el flujo desde la pestaña **Actions** del repositorio. Así puedes confirmar que funciona sin esperar a que Copilot lo ejecute.

## Añade documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información obsoleta o carecen de ella por completo. Esta deuda técnica suele quedar desatendida, lo que reduce la productividad, dificulta el mantenimiento y complica la incorporación de nuevos desarrolladores. Copilot destaca en la generación de documentación, por lo que esta es una tarea ideal para asignar al agente en la nube. Trabajará en segundo plano para crear la documentación necesaria; revisarás el resultado en un ejercicio futuro.

1. En una nueva pestaña del navegador, ve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de creación.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en **Title**.
6. Escribe lo siguiente en **Description**:

   ```plaintext
   Nuestra organización requiere que las funciones y los métodos incluyan comentarios TSDoc cuando sean útiles. Por desgracia, las actualizaciones recientes no han seguido este estándar. Actualiza el código existente para añadir comentarios cuando ayuden a aclarar el comportamiento.
   ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

    ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

    ![Detalles de asignación de Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la PR recién generada, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si aún no aparece, espera uno o dos minutos y actualiza la ventana del navegador.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista de tareas puede tardar varios minutos en aparecer. Copilot está preparando el entorno (ejecutando el flujo de trabajo mencionado antes), analizando el proyecto y determinando el mejor enfoque.

13. Revisa la lista y las tareas que va a completar.
14. Desplázate hacia abajo por la cronología de la PR hasta ver una actualización que indique que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

    ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la ventana para ver el indicador actualizado.

16. Desplázate por la sesión en directo para ver cómo Copilot resuelve el problema: explora el código, entiende el estado del proyecto, se detiene a pensar y decidir un plan y, después, crea código.

Es probable que el trabajo tarde varios minutos. Uno de los objetivos principales del agente en la nube es realizar tareas de forma asíncrona y permitirte centrarte en otras. Aprovecharás esa capacidad asignándole otra tarea y, mientras tanto, escribirás código para añadir funciones a la aplicación.

## Añade una sección de juegos relacionados

Una de las ventajas del agente de GitHub Copilot en la nube es que puedes repartir el trabajo y centrarte en una tarea mientras el agente se ocupa de otra. La sección de juegos relacionados quizá no lleve mucho tiempo, pero ese tiempo se puede dedicar a otros trabajos. Asígnasela al agente en la nube.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de creación.
4. Selecciona **Blank issue**.
5. Escribe `Show related games on the game details page` en **Title**.
6. Escribe lo siguiente en **Description**:

   ```markdown
   Queremos ayudar a los usuarios a descubrir más juegos mostrando títulos relacionados en la página de detalles de cada juego. El equipo de diseño quiere explorar la experiencia de usuario y hacer pruebas de aceptación. Requisitos:

   - Añadir en `src/lib/` una función auxiliar de acceso a datos que devuelva otros juegos de la misma categoría, excluyendo el juego indicado.
   - Mostrar en la página de detalles del juego una sección "Juegos relacionados" que use la función auxiliar.
   - Gestionar el caso en que no haya juegos relacionados.
   - Crear pruebas unitarias para la nueva función auxiliar.
   - Antes de crear la PR, confirmar que se superan todas las pruebas.
   ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

    ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, aparecerá una serie de 👀 en el primer comentario de la incidencia para indicar que Copilot está trabajando.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. El agente en la nube trabaja de manera parecida a un ingeniero de software: no necesitas supervisarlo activamente, sino revisar su trabajo cuando termine. Ahora puedes crear y usar agentes personalizados.

## Resumen y siguientes pasos

Has explorado el agente de [GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona y se ocupe de deuda técnica, funciones o migraciones entre frameworks.

En esta lección has personalizado el entorno de desarrollo, revisado las medidas de seguridad, creado incidencias con un alcance claro y asignado tareas al agente. A continuación, crearás y usarás agentes personalizados. El agente en la nube también admite [servidores MCP][cloud-agent-mcp] e instrucciones personalizadas.

## Recursos

- [Acerca del agente de Copilot en la nube][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
