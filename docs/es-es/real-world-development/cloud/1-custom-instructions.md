---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente de Copilot en la nube →][next-lesson]

El contexto es esencial al trabajar con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información adicional, ese contexto debe estar disponible. Los [archivos de instrucciones][instruction-files] proporcionan directrices para que Copilot entienda no solo *qué* quieres, sino también *cómo* quieres que lo haga.

En este ejercicio:

- explorarás cómo las instrucciones personalizadas del repositorio y las instrucciones específicas de rutas proporcionan contexto, directrices de programación y estándares de documentación.
- añadirás un estándar general a `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los itinerarios de VS Code y CLI, aquí no compararás los resultados de Copilot antes y después de actualizar las instrucciones. El agente de Copilot en la nube trabaja de forma asíncrona con incidencias de GitHub, por lo que es más difícil observar la diferencia en tiempo real. Verás su influencia cuando revises las PR del agente más adelante.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene directrices de desarrollo:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz de usuario debe usar un tema oscuro y tener un aspecto moderno.
- El código debe documentarse mediante comentarios TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que explique su función.

Con los archivos de instrucciones, te asegurarás de que Copilot tenga la información necesaria para realizar el trabajo de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias para que Copilot entienda el estilo de programación y los requisitos. Son una función útil para obtener sugerencias y fragmentos de código más pertinentes. Puedes especificar convenciones, bibliotecas y comentarios, y crear instrucciones para todo el repositorio o para tipos de archivo concretos.

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en cada solicitud del repositorio. Debe contener información del proyecto pertinente para la mayoría de las conversaciones o solicitudes de CLI, como la pila tecnológica, una descripción general de lo que se desarrolla, prácticas recomendadas y directrices generales.
- Los archivos `.github/instructions/*.instructions.md` proporcionan instrucciones para tareas o tipos de archivo concretos. Puedes usarlos para lenguajes como TypeScript o Astro, o para tareas como crear un componente de interfaz o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones se usan para generar código con Copilot Chat, no para las sugerencias de código ni las sugerencias de edición siguiente.
>
> Copilot Chat, Copilot CLI y el agente en la nube utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con frontmatter `applyTo`.
>
> Copilot también admite otros estándares de archivos de instrucciones, como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

No es necesario definir por completo cómo crear archivos de instrucciones en este ejercicio, pero los ejemplos del proyecto muestran un enfoque representativo:

- Mantén `copilot-instructions.md` centrado en directrices del proyecto, como una descripción de lo que se desarrolla, la estructura del proyecto y los estándares generales de programación.
- Usa `*.instructions.md` para directrices específicas de tipos de archivo (pruebas unitarias, componentes de Astro o capa de datos) o tareas concretas.
- Escribe en lenguaje natural. Expón las directrices con claridad e incluye ejemplos del código que quieres y del que no quieres.

No hay una única forma correcta de crear archivos de instrucciones ni de usar IA. La experimentación te ayudará a encontrar el enfoque que mejor se adapte al proyecto.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían tener un conjunto sólido de archivos de instrucciones. En este proyecto verás instrucciones para muchas tareas, incluidas las [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta opción de una manera distinta (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI); el ejercicio correspondiente lo indicará.
>
> ¿Buscas plantillas o un punto de partida? Explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses

## Explora los archivos de instrucciones del proyecto

Revisa el `copilot-instructions.md` principal y los archivos `*.instructions.md` incluidos en el repositorio.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y observa su breve descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en las indicaciones anidadas para los flujos de trabajo de GitHub Actions. Se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explora su contenido. Verás instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y más.
4. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` del principio define un patrón relativo a la raíz del repositorio; en este caso, coincide con cualquier archivo de prueba TypeScript, como `**/*.test.ts`.
5. Revisa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los vínculos a otros archivos de instrucciones (como `unit-tests.instructions.md`) y a archivos existentes del proyecto. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero aún no exige documentación dentro del código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y los encabezados de comentario.

## Añade un estándar del repositorio

El siguiente paso es la única edición que harás en este ejercicio: añadir una regla general para que la documentación esté en el código en forma de comentarios TSDoc y un encabezado de comentario a nivel de archivo. El agente en la nube aplicará esta regla al trabajar en las incidencias que le asignes más adelante.

Antes de editar, crea una rama. El agente en la nube leerá las instrucciones de la rama de destino de la incidencia, pero confirmarás tus cambios de forma ordenada.

1. En el terminal del codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Una vez preparada la rama, abre de nuevo `.github/copilot-instructions.md`. Este archivo proporciona a Copilot información general del proyecto; añade un estándar de programación para mejorar las sugerencias de código.

1. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Describe los estándares del proyecto, pero aún no exige documentación en el código; por eso la función auxiliar generada no incluía comentarios.
2. Añade estas líneas de Markdown justo debajo de los estándares existentes:

   ```markdown
   - Todas las funciones exportadas deben tener un comentario TSDoc que describa su propósito, sus parámetros y su valor de retorno.
   - Antes de las importaciones y del resto del código, añade un bloque de comentarios que explique el propósito del archivo.
   ```

3. Guarda `copilot-instructions.md`.

## Confirma, publica y fusiona las instrucciones

El agente en la nube lee las instrucciones de la rama a la que se dirige la incidencia. En el ejercicio siguiente le asignarás incidencias de `main`, así que las instrucciones deben llegar a `main`.

1. Prepara y confirma el archivo:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Publica la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En GitHub.com, abre una PR desde `custom-instructions-cloud` a `main` y fusiónala.

> [!TIP]
> Como viste en la lección anterior, los archivos de instrucciones pueden estar en el nivel del repositorio (`.github/copilot-instructions.md`) para proporcionar directrices generales o en archivos `*.instructions.md` para lenguajes, tipos de archivo y tareas concretas. El archivo del repositorio es el lugar adecuado para estándares generales, como el requisito de comentarios de documentación.
>
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir la rama y confirmar los cambios en `main`. La rama permite practicar el flujo habitual de un proyecto real.

## Resumen y siguientes pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones y has añadido un estándar general a `.github/copilot-instructions.md`. Lo verás en las PR que genere el agente en la nube durante los ejercicios restantes.

A continuación, [asignarás tu primera incidencia al agente de Copilot en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para redactar mejores instrucciones de Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
