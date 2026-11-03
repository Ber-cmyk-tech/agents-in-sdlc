---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este laboratorio has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funciones. Le has pedido que documente el código, cree una función de juegos relacionados para que el equipo de diseño pueda iterar y añada funciones de accesibilidad, incluidos controles para los modos de contraste alto y claro. Ahora revisarás los cambios de código y, si hace falta, pedirás a Copilot que los mejore antes de habilitar la revisión para el resto del equipo.

### Escenario

Como ya hemos señalado varias veces, los fundamentos del diseño de software y DevOps no cambian con la IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Con eso en mente, revisaremos las sugerencias de GitHub Copilot para la documentación, la función de juegos relacionados y las mejoras de accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y agente de GitHub Copilot en la nube

El agente de Copilot en la nube realiza tareas de forma asíncrona y sin supervisión, así que se han establecido medidas de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utilizará para el código.
- El agente se ejecuta en GitHub Actions, dentro de un entorno efímero independiente.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de que puedan ejecutarse.
- [El acceso a recursos externos está limitado de forma predeterminada][agent-firewall], incluidos los servidores MCP.

## Revisa la documentación generada

Empieza por la primera solicitud de incorporación de cambios (PR) creada por el agente de Copilot en la nube: la que añade documentación al código. Usarás la interfaz estándar de PR de GitHub.com.

> [!NOTE]
> Al revisar la PR, puede que aparezca una advertencia que indica que el cortafuegos bloqueó a GitHub Copilot. Es lo esperado, ya que el acceso a recursos externos, incluidas las llamadas a servidores MCP externos, está limitado de forma predeterminada. Si quieres, puedes [personalizar o deshabilitar el cortafuegos del agente en la nube][agent-firewall].

1. Vuelve al repositorio en GitHub.com.
2. Selecciona **Pull Requests** para abrir la lista de PR.
3. Abre la solicitud cuyo título se parezca a **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, la PR tendrá la marca **[WIP]**. Espera a que termine; puede tardar varios minutos. Mientras tanto, puedes descansar o repasar lo que has aprendido.

4. Cuando la PR esté lista, selecciona **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye los nuevos comentarios TSDoc y otra documentación. Los cambios concretos pueden variar.

   Mientras revisas, busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentario. Proceden de los archivos de instrucciones que revisaste al principio del Ejercicio 2.

6. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Comprueba si hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Comprueba que los flujos de trabajo se han puesto en cola en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. Esto puede tardar unos minutos.

## Pide cambios a GitHub Copilot

Trabajar con Copilot en una PR no es un proceso unidireccional. Puedes mencionarlo en comentarios de la PR, como harías con otra persona del equipo, o añadir comentarios en línea en el código. Copilot verá el comentario e iniciará otra sesión para resolverlo. Como los resultados no son deterministas, no hay una indicación exacta que debas usar. Puedes pedirle, por ejemplo, que:

- añada al principio de cada archivo de código encabezados de comentario con una descripción breve.
- añada comentarios TSDoc a archivos TypeScript y Astro.
- cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario para pedir un cambio en la documentación generada y menciona a **@copilot**, como mencionarías a cualquier usuario. Usa una de las ideas anteriores o pide otro cambio de documentación que quieras ver.
2. Selecciona **View Session** para observar el trabajo de Copilot. Verás que inicia una sesión nueva para realizar los cambios.
3. Selecciona **Back to pull request** para volver a la solicitud.

   ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, aparecerá una nueva confirmación en la PR.
5. Selecciona **Files changed** y revisa los cambios.

Puedes seguir iterando hasta que estés satisfecho. Cuando lo estés, marca la PR como lista para revisión y fusiónala en la rama principal.

![Cambiar la PR a lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la función de juegos relacionados

Vuelve a la PR que Copilot creó para resolver la incidencia de juegos relacionados en la página de detalles.

1. Ve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Abre la PR cuyo título se parezca a **Show related games on the game details page**.
4. Selecciona **Files changed** para revisar el código generado.
5. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
6. Comprueba si hay flujos de trabajo pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Comprueba que los flujos de trabajo se han puesto en cola en la sección de comprobaciones de la PR y que se superan las comprobaciones del proyecto para la aplicación Astro. Puede tardar unos minutos.
9. **Opcional:** cambia a esta rama en tu Codespace y prueba manualmente la función de juegos relacionados. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, `copilot/fix-8`:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ha creado la función de juegos relacionados. Como antes, puedes iterar y pedirle que cambie, por ejemplo, cuántos juegos relacionados aparecen o que añada encabezados de comentario y comentarios TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas. Añade la solicitud como comentario en **Conversation**; Copilot la verá e iniciará otra sesión.

## Revisa las funciones de accesibilidad

Por último, revisa las funciones implementadas con el agente personalizado de accesibilidad. Esta PR debe incluir tanto el modo de contraste alto del Ejercicio 3 como el modo claro que pediste desde el centro de control en el Ejercicio 4.

1. Ve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Abre la PR cuyo título se parezca a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando, la PR tendrá la marca **[WIP]**. Espera a que termine; puede tardar varios minutos.

4. Selecciona **Files changed** y revisa el código generado.
5. Revisa la implementación y presta atención a:
   - los controles para cambiar de modo.
   - el uso del almacenamiento local para guardar las preferencias.
   - los cambios de CSS o estilos para los modos de contraste alto y claro.
   - los atributos de accesibilidad, como las etiquetas ARIA y la navegación por teclado.
   - el código JavaScript o TypeScript que gestiona el cambio de modo.
6. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
7. Comprueba si hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Comprueba que los flujos de trabajo se han puesto en cola en la sección de comprobaciones de la PR y que se superan las comprobaciones del proyecto para la aplicación Astro. Puede tardar unos minutos.
10. **Opcional:** cambia a esta rama en tu Codespace para probar manualmente las funciones de accesibilidad. Abre el terminal y ejecuta los comandos siguientes; sustituye `<branch-name>` por el nombre de la rama de Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los controles de contraste alto y modo claro en el navegador. Confirma que funcionan como se espera y que las preferencias persisten al recargar la página.

Observa cómo el agente personalizado de accesibilidad ha orientado a Copilot para implementar estas funciones siguiendo las prácticas recomendadas. Si detectas problemas de accesibilidad o mejoras posibles, menciona a **@copilot** en un comentario, como hiciste con las PR anteriores, para pedir una actualización.

## Ejercicio opcional: sigue delegando

El agente en la nube funciona mejor cuando puedes asignarle tareas reales del trabajo pendiente y ocuparte de otra cosa. Para practicarlo, crea algunas incidencias más en el repositorio y asígnaselas a Copilot. Algunas ideas:

- crear un formulario de interés de patrocinadores en la página de detalles del juego.
- implementar paginación en la página de lista de juegos.
- añadir validación de entradas y gestión de errores a las funciones auxiliares de acceso a datos.

## Resumen

Has completado el itinerario del agente en la nube. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizadas** del repositorio para observar más adelante su efecto en el trabajo del agente.
- **Has asignado incidencias al agente de Copilot en la nube** y has visto cómo prepara el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y usado un agente personalizado** de accesibilidad para añadir controles de contraste alto y modo claro.
- **Has usado la página de agentes de Copilot como centro de control** para supervisar y dirigir la sesión de accesibilidad en curso.
- **Has revisado e iterado sobre las PR del agente en la nube**, mencionando a `@copilot` para solicitar cambios y aprobando los flujos de trabajo.

## Revisión y siguientes pasos

Has completado el itinerario del agente en la nube. Los otros itinerarios complementan lo que has practicado:

- 🖥️ **[Itinerario de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración MCP directamente desde el IDE.
- 💻 **[Itinerario de CLI](../../cli/)**: publica dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y el puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, puedes asignar al agente una incidencia de refactorización o cobertura de pruebas, crear un agente personalizado para otro ámbito o configurar `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el cortafuegos del agente en la nube][agent-firewall]

---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
