---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo de agente →][next-lesson]

El contexto es esencial al trabajar con IA generativa. Si una tarea debe hacerse de una forma concreta o Copilot necesita información adicional, ese contexto debe estar disponible. Los [archivos de instrucciones][instruction-files] permiten proporcionar indicaciones sobre cómo generar código.

En este ejercicio:

- explorarás cómo las instrucciones personalizadas del repositorio y las instrucciones específicas de rutas proporcionan contexto del proyecto, directrices de programación y estándares de documentación.
- generarás la primera parte de la capa de datos para el filtrado (una función auxiliar de editores) con las instrucciones actuales.
- añadirás un nuevo estándar general a `.github/copilot-instructions.md`.
- volverás a enviar el mismo mensaje y observarás cómo el código generado sigue el nuevo estándar.
- confirmarás los cambios de instrucciones y el filtrado en `main` para que Copilot use las nuevas directrices en el ejercicio siguiente.

> [!CAUTION]
> El código generado puede no seguir todos los estándares que hayas definido. Copilot es probabilístico. El objetivo es observar la tendencia tras actualizar las instrucciones, no obtener siempre el mismo resultado exacto.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene directrices y requisitos de desarrollo, como:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz de usuario debe usar un tema oscuro y tener un aspecto moderno.
- El código debe documentarse mediante comentarios TSDoc.
- Al inicio de cada archivo debe haber un bloque de comentarios que describa su función.

Usarás archivos de instrucciones para que Copilot disponga de la información adecuada al realizar tareas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias para que Copilot entienda el estilo de programación y los requisitos. Puedes especificar convenciones, bibliotecas y los comentarios que quieres incluir. Las instrucciones pueden aplicarse a todo el repositorio o a determinados tipos de archivo.

Hay dos tipos:

- `.github/copilot-instructions.md`: un único archivo que se envía a Copilot en cada solicitud del repositorio. Debe contener información pertinente para la mayoría de las solicitudes, como la pila tecnológica, una descripción general del proyecto y las prácticas recomendadas.
- `.github/instructions/*.instructions.md`: archivos para tareas o tipos de archivo concretos, como TypeScript, componentes de Astro o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones se usan para generar código con Copilot Chat, no para las sugerencias de código ni las sugerencias de edición siguiente.
>
> Copilot Chat, Copilot CLI y el agente en la nube utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con frontmatter `applyTo`.
>
> Copilot también admite otros estándares de archivos de instrucciones, como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

No es necesario definir por completo cómo crear archivos de instrucciones en este ejercicio, pero los ejemplos del proyecto muestran un enfoque representativo:

- Mantén `copilot-instructions.md` centrado en las directrices del proyecto, como una descripción de lo que se desarrolla, la estructura del proyecto y los estándares generales de programación.
- Usa `*.instructions.md` para directrices específicas de tipos de archivo (pruebas unitarias, componentes de Astro o capa de datos) o tareas concretas.
- Escribe en lenguaje natural. Expón las directrices con claridad e incluye ejemplos del código que quieres y del que no quieres.

No hay una única forma de crear archivos de instrucciones ni de usar IA. La experimentación te ayudará a encontrar el enfoque que mejor se adapte al proyecto.

> [!TIP]
> Los proyectos con GitHub Copilot deberían tener un conjunto sólido de archivos de instrucciones. En este proyecto encontrarás archivos para [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions]. Copilot también puede ayudar a generarlos: por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. Para consultar plantillas, explora [awesome-copilot][awesome-copilot].

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization

## Explora los archivos de instrucciones personalizadas del proyecto

El repositorio incluye un `copilot-instructions.md` principal y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Revisa la descripción breve del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en las indicaciones anidadas para los flujos de trabajo de GitHub Actions.
3. Explora la carpeta `.github/instructions`, que incluye instrucciones para Astro, la capa de datos Drizzle, pruebas y más.
4. Abre `.github/instructions/unit-tests.instructions.md` y revisa el campo `applyTo`, que define el patrón de archivos al que se aplican. En este caso, coincide con archivos de prueba TypeScript como `**/*.test.ts`.
5. Revisa las instrucciones específicas para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y revisa los vínculos a otros archivos, como `unit-tests.instructions.md`, y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares del proyecto, pero aún no exige documentación dentro del código. En los pasos siguientes añadirás esa regla.

## Crea una rama para los cambios

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea y cambia a una rama nueva:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat antes de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, abre Copilot Chat en el codespace y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo dependen del ejercicio; se indicarán cuando corresponda.

Primero enviarás un mensaje con las instrucciones actuales y, después de actualizarlas, repetirás el mismo mensaje para comparar el resultado.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot use solo el contexto necesario.
2. Abre `src/lib/publishers.ts` para indicar dónde debe ubicarse la función auxiliar.
3. En la vista Chat, selecciona **Agent** en el selector de agentes.
4. Envía este mensaje:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con una función auxiliar de acceso a datos que devuelva una lista de todos los editores, con el nombre y el id de cada uno. Aplica los cambios al archivo.
   ```

5. Copilot explorará el proyecto y aplicará cambios, normalmente al archivo de la función auxiliar y a sus pruebas.
6. Observa que la función propuesta recibe como primer argumento un cliente `db` y devuelve una matriz de editores tipada. Esto se debe a las convenciones de capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplican a `src/lib/*.ts`.
7. Observa que el código propuesto **no incluye** comentarios TSDoc ni un comentario de cabecera para el archivo.

> [!CAUTION]
> Como Copilot es probabilístico, puede que añada comentarios aunque no se le hayan pedido. No pasa nada; lo importante es observar si mejora la coherencia tras actualizar las instrucciones.

## Añade un nuevo estándar del repositorio

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Aunque documenta los estándares de programación, aún no incluye documentación en el código.
3. Añade estas líneas de Markdown debajo de los estándares existentes:

   ```markdown
   - Todas las funciones exportadas deben tener un comentario TSDoc que describa su propósito, sus parámetros y su valor de retorno.
   - Antes de las importaciones y del resto del código, añade un bloque de comentarios que explique el propósito del archivo.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Los archivos de instrucciones pueden aplicarse a todo el repositorio (`.github/copilot-instructions.md`) o a lenguajes, tipos de archivo y tareas concretas (`*.instructions.md`). Las normas generales de documentación del código corresponden al archivo del repositorio.

## Repite el mensaje y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat**.
2. Vuelve a `src/lib/publishers.ts`.
3. Envía el mismo mensaje:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con una función auxiliar de acceso a datos que devuelva una lista de todos los editores, con el nombre y el id de cada uno. Aplica los cambios al archivo.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Funciones auxiliares de acceso a datos de editores para la plataforma de financiación colectiva Tailspin Toys.
    * Proporciona funciones para recuperar información de los editores de la base de datos.
    */
   ```

5. Observa que la función ahora incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Devuelve una lista de todos los editores con su id y nombre.
    *
    * @param db - Cliente de base de datos Drizzle.
    * @returns Promesa que se resuelve en una matriz de objetos de editor.
    */
   ```

Has orientado a Copilot para que siga un estándar del proyecto y lo aplique al código que se utilizará en el ejercicio siguiente.

## Confirma las instrucciones y publica la rama

1. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Confirma que están presentes las actualizaciones de instrucciones y de la función auxiliar con `git status`.
3. Prepara y confirma las instrucciones y todos los archivos que Copilot haya modificado para la función auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios relacionados, como pruebas, añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Publica la rama:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y fusiona una solicitud de incorporación de cambios

La plantilla del proyecto ya conecta GitHub con el espacio de trabajo. Pide a Copilot que encuentre la incidencia sobre las instrucciones y cree una PR que la cierre e incluya la función auxiliar de editores.

1. Abre Copilot Chat en el codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista, y confirma que **Agent** está seleccionado.
3. Envía este mensaje:

   ```plaintext
   Busca la incidencia relacionada con la actualización del archivo de instrucciones. Crea una PR desde la rama actual, indica que cierra esa incidencia e incluye que también hemos añadido la base de la función auxiliar de editores para el próximo trabajo de filtrado.
   ```

4. Si Copilot te pide permiso para usar herramientas de GitHub, revisa la solicitud y selecciona **Allow** cuando corresponda.
5. Cuando cree la PR, pídele que la fusione en `main` y actualice tu copia local:

   ```plaintext
   Fusiona la PR en main. Después, vuelve localmente a main y descarga el código más reciente.
   ```

6. Revisa las solicitudes de permiso de GitHub y del terminal antes de seleccionar **Allow**.

## Resumen y siguientes pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones y, con Copilot Chat en VS Code, has:

- enviado un mensaje de generación de código y observado el resultado con las instrucciones actuales.
- añadido un nuevo estándar general a `.github/copilot-instructions.md`.
- repetido el mensaje y observado cómo el código propuesto seguía el nuevo estándar.
- confirmado las instrucciones y la función auxiliar en `main` para continuar el trabajo en el ejercicio siguiente.

A continuación, aplicarás esas instrucciones en el [modo de agente][next-lesson] mientras Copilot añade una función en distintos puntos del código.

## Recursos

- [Instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para redactar mejores instrucciones de Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
