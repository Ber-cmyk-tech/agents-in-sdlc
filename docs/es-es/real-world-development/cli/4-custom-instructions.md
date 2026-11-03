---
title: "Lección 4 - Guiar a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es esencial al trabajar con IA generativa. Si una tarea debe realizarse de una forma concreta, proporciona a Copilot las instrucciones necesarias. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ahora que has creado el filtrado, explorarás las instrucciones que ha seguido Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas de rutas.
- actualizarás las instrucciones para que se sigan los estándares de programación.
- observarás el efecto de los archivos de instrucciones en el código.

## Escenario

Tailspin Toys tiene directrices de desarrollo, como:

- Los comentarios deben explicar la intención y las decisiones que no sean obvias, no repetir el código.
- Las funciones exportadas en `db/` y `src/lib/` deben documentar con TSDoc/JSDoc su propósito, parámetros y valores de retorno, incluido el argumento `db` inyectable cuando corresponda.
- Los componentes reutilizables de Astro deben documentar sus contratos `Props`, y los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Se deben conservar las directrices de formato y lint existentes.

Usarás archivos de instrucciones para proporcionar a Copilot el contexto necesario para seguir estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan contexto y preferencias para que Copilot entienda el estilo de programación y los requisitos. Puedes especificar convenciones, bibliotecas y comentarios, y aplicarlas a todo el repositorio o a determinados tipos de archivo.

- `.github/copilot-instructions.md`: un único archivo que se envía a Copilot en cada solicitud del repositorio y contiene directrices generales.
- `.github/instructions/*.instructions.md`: archivos con indicaciones específicas para lenguajes, tipos de archivo o tareas.

> [!NOTE]
> La compatibilidad con otros formatos de instrucciones depende del entorno. Consulta la [referencia de formatos compatibles][custom-instructions-support] antes de depender de uno.

## Explora los archivos de instrucciones personalizadas

El proyecto de inicio ya incluye varios archivos de instrucciones. Revisa su contenido antes de hacer cambios para observar el efecto.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces (no en el terminal), abre `.github/copilot-instructions.md`.
3. Revisa la descripción breve del proyecto y las directrices de programación. Se aplican a todas las interacciones con Copilot en este repositorio.
4. Explora `.github/instructions`, donde encontrarás instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y otras tareas.
5. Abre `.github/instructions/unit-tests.instructions.md` y revisa el campo `applyTo`, que define el patrón de archivos al que se aplican.
6. Abre `.github/instructions/drizzle.instructions.md` y observa sus vínculos a otros archivos de instrucciones y a archivos del proyecto. Así puedes dividir las instrucciones en unidades reutilizables y ofrecer ejemplos.

## Actualiza las instrucciones para reflejar las directrices del equipo

Modifica `.github/copilot-instructions.md` para asegurar que el nuevo código TypeScript incluya comentarios TSDoc.

1. Busca la sección **Code formatting guidance**, cerca de la línea 35.
2. Añade este elemento al final de la lista:

   ```markdown
   - Todo el código TypeScript nuevo debe incluir comentarios TSDoc con fines de documentación.
   ```

El archivo se guardará automáticamente.

## Usa las instrucciones actualizadas

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Cierra y reanuda la conversación de filtrado para que Copilot use las nuevas instrucciones sin perder el contexto de la función.

1. Si la conversación de filtrado sigue abierta, escribe `/exit`. Copilot CLI guarda automáticamente las conversaciones.
2. Desde la raíz del repositorio, en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está seleccionada la rama `game-filters-cli`.
3. Pide a Copilot que aplique las directrices actualizadas:

   ```plaintext
   Acabamos de actualizar nuestras instrucciones y las directrices de código. ¿Puedes actualizar el código que has generado para que las siga?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Observa los comentarios TSDoc y confirma que describen el código correctamente.

## Resumen y siguientes pasos

Has explorado cómo Copilot CLI obtiene contexto de los archivos de instrucciones y has aplicado un nuevo estándar a la función. En concreto, has explorado instrucciones del repositorio y específicas de rutas, las has actualizado para incluir una norma de programación y has observado su efecto.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable `quality-checks`][next-lesson] para asegurar que el lint y las pruebas se ejecuten de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas a Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: archivos de instrucciones][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
