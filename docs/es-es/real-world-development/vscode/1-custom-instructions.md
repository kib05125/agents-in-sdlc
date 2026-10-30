---
title: "Ejercicio 1: Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo de agente →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta, o Copilot necesita conocer información de contexto, debes asegurarte de que pueda acceder a ella. Hay varias formas de compartir contexto específico con Copilot. Una de las principales son los [archivos de instrucciones][instruction-files], que proporcionan directrices para la generación de código.

En este ejercicio, aprenderás a:

- explorar cómo las instrucciones personalizadas del repositorio y los archivos de instrucciones específicos para determinadas rutas proporcionan a Copilot contexto del proyecto, directrices de programación y estándares de documentación;
- generar la primera parte de la capa de datos para los filtros (un helper de editoriales) con las instrucciones actuales;
- añadir un nuevo estándar para todo el repositorio a `.github/copilot-instructions.md`;
- volver a enviar la misma solicitud y observar cómo el código generado adopta el nuevo estándar;
- confirmar la actualización de las instrucciones y el trabajo de filtros en `main` para que Copilot utilice las directrices actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir algunos de los estándares que hayas definido. Copilot no es determinista. El objetivo del ejercicio es observar la tendencia en el comportamiento después de actualizar las instrucciones, no obtener exactamente el mismo código carácter por carácter.

## Archivos de instrucciones

### Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo. Entre ellos se incluyen:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe utilizar el modo oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que describa su función.

Con los archivos de instrucciones, te asegurarás de que Copilot disponga de la información adecuada para realizar las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para que pueda comprender mejor tu estilo y tus requisitos de programación. Te permiten orientar a Copilot para que genere sugerencias y fragmentos de código más pertinentes. Puedes especificar las convenciones de programación, las bibliotecas e incluso los comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información pertinente para la mayoría de las solicitudes de chat o CLI, como la pila tecnológica, una descripción general del proyecto, las prácticas recomendadas y otras directrices globales.
- Los archivos `.github/instructions/*.instructions.md` proporcionan directrices para tareas o tipos de archivo concretos. Puedes definir pautas para lenguajes como TypeScript o Astro, o para tareas como crear un componente de interfaz de usuario o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se utilizan para generar código en Copilot Chat, no para completar código ni sugerir ediciones posteriores.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con metadatos `applyTo`.
>
> Copilot también [admite archivos de instrucciones basados en otros estándares][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Hablar en detalle sobre cómo crear archivos de instrucciones queda fuera del alcance del taller. Sin embargo, los ejemplos del proyecto muestran un enfoque representativo. En general:

- Mantén `copilot-instructions.md` centrado en las directrices del proyecto, como su descripción, estructura y estándares globales de programación.
- Usa archivos `*.instructions.md` para dar instrucciones específicas sobre tipos de archivo (pruebas unitarias, componentes Astro, capa de datos) o tareas concretas.
- Escribe en lenguaje natural y mantén las indicaciones claras. Incluye ejemplos del aspecto que debe y no debe tener el código.

No hay una única forma de crear archivos de instrucciones, al igual que no hay una única forma de usar la IA. La experimentación te ayudará a averiguar qué funciona para tu proyecto.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían contar con un conjunto sólido de archivos de instrucciones. Al explorar los archivos de este proyecto, verás que hay instrucciones para muchos tipos de tareas, como las [actualizaciones de la interfaz de usuario][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta opción de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. La lección del entorno que estés usando lo indicará cuando corresponda.
>
> ¿Buscas plantillas o un punto de partida? Explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Dedica un momento a leer los archivos de instrucciones incluidos en este repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas para **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot en este repositorio.
3. Abre la carpeta `.github/instructions` y explora sus archivos. Hay instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y más.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` del encabezado: define un patrón glob relativo a la raíz del repositorio que determina a qué archivos se aplican las instrucciones. En este caso, coincide con cualquier archivo de pruebas TypeScript, por ejemplo, uno que se ajuste a `**/*.test.ts`.
5. Lee las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones (como `unit-tests.instructions.md`) y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y señalar a Copilot ejemplos que puede seguir al generar código. (Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio).

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero aún no incluye requisitos sobre la documentación en el código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y los encabezados de comentario de los archivos.

## Crea una rama para los cambios

Vamos a seguir algunas prácticas recomendadas y crear una rama para los cambios.

1. Vuelve al Codespace del ejercicio anterior.
2. Abre un terminal nuevo pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat antes de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al Codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea necesario.

Para observar el efecto de las instrucciones personalizadas, empieza enviando una solicitud con el archivo de instrucciones actual. Más adelante, actualízalo y vuelve a enviar la misma solicitud para comparar los resultados.

1. Cierra las pestañas abiertas del editor de ejercicios anteriores para que Copilot solo utilice el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ubicarse el helper.
3. En el menú desplegable de agentes de la vista Chat, selecciona **Agent** para que Copilot pueda aplicar cambios a los archivos.

   ![Captura de pantalla del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envía esta solicitud:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará actualizaciones al código, a menudo en el archivo helper y en sus pruebas.
6. Observa que el helper propuesto es una función tipada que recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editoriales. Esto procede de las convenciones de la capa de datos definidas en `.github/instructions/drizzle.instructions.md`, que se aplican a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** los comentarios TSDoc y el encabezado de comentario a nivel de archivo.

> [!CAUTION]
> Como Copilot no es determinista, puede que añada comentarios de documentación aunque no se lo pidas. No pasa nada; lo importante es observar cómo mejora la coherencia tras actualizar las instrucciones.

## Añade un nuevo estándar para el repositorio

Como se ha indicado, `.github/copilot-instructions.md` proporciona información del proyecto a Copilot. Documentemos los estándares de programación del repositorio para mejorar las sugerencias.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que documenta los estándares de programación del proyecto, pero no incluye reglas para la documentación en el código; por eso el helper generado no tenía comentarios de documentación.
3. Añade las siguientes líneas de Markdown justo después de los estándares existentes. Así indicarás a Copilot que añada encabezados de comentario y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, puedes crear archivos de instrucciones en el nivel del repositorio (`.github/copilot-instructions.md`) para proporcionar directrices globales, o usar archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. El archivo del repositorio es el lugar adecuado para estándares globales, como la regla sobre los comentarios de documentación que acabas de añadir.

## Vuelve a enviar la solicitud y observa los cambios

1. Vuelve a Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva.
2. Selecciona `src/lib/publishers.ts` en el editor para que Copilot se centre en el archivo correcto.
3. Envía **la misma solicitud** que antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentario parecido a este:

   ```typescript
   /**
    * Helpers de acceso a datos de editoriales para la plataforma de financiación colectiva Tailspin Toys.
    * Proporciona funciones para recuperar información de editoriales de la base de datos.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Devuelve una lista de todas las editoriales con su identificador y nombre.
    *
    * @param db - Cliente de base de datos Drizzle.
    * @returns Una promesa que se resuelve en una matriz de objetos de editoriales.
    */
   ```

Has orientado a Copilot para que siga un nuevo estándar y lo aplique al código real sobre el que se basará el siguiente ejercicio.

## Confirma las instrucciones y publica la rama

Los archivos de instrucciones son recursos del repositorio y se gestionan como cualquier otro archivo mediante el control de código fuente. Vamos a confirmar los cambios y publicar la rama.

1. Abre una nueva ventana del terminal pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. En el terminal, confirma que están presentes las actualizaciones de instrucciones y del helper ejecutando:

   ```bash
   git status
   ```

3. En el terminal, prepara y confirma la actualización de las instrucciones y todos los archivos modificados por Copilot para crear la base del helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otras actualizaciones de apoyo (por ejemplo, pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Publica la rama en el repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios

Una vez publicada la rama, abre una solicitud de incorporación de cambios y vincúlala a la incidencia del backlog sobre la actualización de los estándares de documentación. Copilot puede buscar la incidencia, crear la solicitud para cerrarla y, después, combinarla.

> [!NOTE]
> En este ejercicio Copilot actuará por primera vez en GitHub. La plantilla del proyecto ya conecta las herramientas de GitHub con el espacio de trabajo, así que no hay que configurar nada. La primera vez que Copilot utilice una herramienta, es posible que VS Code te pida iniciar sesión en GitHub. Sigue las instrucciones para permitirlo. En un ejercicio posterior aprenderás cómo funciona esta conexión (Model Context Protocol).

1. Abre Copilot Chat en tu Codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y comprueba que **Agent** está seleccionado en el selector de agentes.
3. Pide a Copilot que busque la incidencia relacionada con la actualización de los archivos de instrucciones y que cree una PR desde la rama actual. Indícale que describa tanto la actualización de las instrucciones como la nueva base del helper de editoriales:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Cuando Copilot te pida permiso para realizar acciones en GitHub, revisa el comando y selecciona **Allow** si corresponde.
5. Cuando se cree la PR, pide a Copilot que la combine y vuelva a `main`:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Cuando Copilot te pida permiso para realizar acciones en GitHub y ejecutar comandos de shell, revísalos y selecciona **Allow** si corresponde.

¡Has creado y combinado una solicitud de incorporación de cambios con la ayuda de GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones del proyecto y has usado Copilot Chat en VS Code para:

- enviar una solicitud de generación de código y observar el resultado con las instrucciones *actuales*;
- añadir un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`;
- volver a enviar la misma solicitud y observar cómo el código propuesto adopta el nuevo estándar;
- confirmar las instrucciones y la base del helper en `main` para que el siguiente ejercicio pueda aprovecharlas.

A continuación, aplicarás esas instrucciones en el [modo de agente][next-lesson] mientras Copilot añade una funcionalidad al código base.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
