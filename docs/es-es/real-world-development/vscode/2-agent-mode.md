---
title: "Ejercicio 2: Añadir funcionalidad con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones como ejecutar pruebas. El flujo de trabajo habitual de un desarrollador consiste en localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo se ha pasado por alto, volver a actualizarlo... y así sucesivamente.

Para eso sirve el modo de agente de Copilot.

El modo de agente de Copilot está diseñado para trabajar de forma más autónoma en el IDE. Se comporta de manera parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Vamos a ver cómo usarlo para añadir nuevas funciones al sitio.

> [!NOTE]
> Aunque los nombres son parecidos, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en el IDE, permite recibir comentarios rápidamente e interactuar con él. El agente en la nube trabaja de forma asíncrona como un miembro del equipo y colabora contigo a través de incidencias y solicitudes de incorporación de cambios.

En este ejercicio, aprenderás a:

- utilizar el modo de agente de Copilot para explorar el proyecto, identificar los archivos pertinentes y coordinar cambios;
- implementar funcionalidades en la interfaz de usuario y la capa de datos con el modo de agente de GitHub Copilot;
- revisar los cambios y las pruebas generados por el modo de agente de Copilot antes de combinarlos en el código base.

## Escenario

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editorial. En el ejercicio anterior ya añadiste un helper para las editoriales. Ahora completarás el trabajo restante en la capa de datos, la interfaz de usuario y las pruebas con el modo de agente de Copilot.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, exploremos el sitio web de Tailspin Toys para conocer sus funciones actuales.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar una lista de juegos y ver sus detalles. El sitio es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local desde sus metadatos mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz de usuario en el cliente. Los helpers de acceso a datos reutilizables están en `src/lib/`, y la interactividad se implementa con scripts `<script>` pequeños y específicos de Astro que utilizan las API DOM estándar.

### Inicia el sitio web

Para facilitar el inicio del sitio, se ha definido la tarea `npm run dev`, que ejecuta el único servidor de desarrollo de Astro. Puedes ejecutarla en tu GitHub Codespace siguiendo estos pasos:

1. Vuelve al Codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal nuevo en el Codespace pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama para el trabajo con los filtros y cámbiate a ella:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta el siguiente comando para iniciar el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, aparecerá un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si se abre un cuadro de diálogo para preguntarte si quieres abrir una ventana del navegador para `http://localhost:4321`, ciérralo seleccionando la **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando en el terminal la dirección del servidor Astro `http://localhost:4321`.

> [!NOTE]
> En un Codespace, al seleccionar un enlace localhost desde el terminal se te redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado a tu Codespace, que ahora aloja el servidor web.

### Explora el sitio web

Cuando el sitio esté en ejecución, podrás explorar sus funciones principales:

- **Página de inicio**: muestra una lista de juegos de mesa con títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al Codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea necesario.

La implementación inicial del sitio funciona, pero queremos añadir capacidades. Empecemos por revisar el backlog. Al crear el proyecto a partir de la plantilla, se generó automáticamente un backlog de incidencias. Pide a GitHub Copilot que te muestre esos elementos.

1. En el menú desplegable de agentes de la vista Chat, selecciona **Agent**. El agente planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos de terminal y utiliza herramientas.

   ![Captura de pantalla del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no prefieren un modelo sobre otro. Durante la creación del taller usamos Claude Sonnet 4.5 y lo incluimos en las instrucciones para que las sugerencias de código sean relativamente coherentes y la experiencia sea adecuada. Sin embargo, como los LLM son probabilísticos, es posible que recibas sugerencias distintas a las que se muestran en el taller. Es normal y esperable.

> [!NOTE]
> Debido a la naturaleza probabilística de los LLM, Copilot puede utilizar un comando MCP diferente, pero debería poder completar la tarea.

3. Envía a Copilot esta solicitud sobre el backlog de incidencias:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista de incidencias generada.

Observa que Copilot también ha priorizado los elementos según cuáles cree que serán más útiles para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedirle al agente que genere código, revisa el archivo de instrucciones que dará contexto a Copilot. Utilizarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio.

1. En el Codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa las directrices generales para añadir funcionalidad. Incluyen:
   - una descripción general de la arquitectura;
   - principios de diseño de componentes, capacidad de prueba y accesibilidad;
   - enlaces a archivos de instrucciones específicos para varios tipos de archivo, entre ellos:
     - Astro;
     - Tailwind CSS;
     - Drizzle.

> [!TIP]
> Los archivos de instrucciones permiten hacer referencia tanto a otros archivos de instrucciones como a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Así puedes reutilizar contenido, dividir conjuntos complejos en partes más pequeñas y manejables y proporcionar ejemplos y plantillas.

## Implementa los filtros

Para completar los filtros, tendrás que realizar al menos tres cambios independientes en la aplicación:

- añadir o ajustar la lógica restante de filtrado en la capa de datos (`src/lib/`);
- añadir o actualizar las pruebas del comportamiento de los filtros;
- actualizar la página con la lista de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y pasar antes de combinar los cambios en el código base. El modo de agente de Copilot puede encargarse de estas tareas. Vamos a añadir la funcionalidad.

1. Puedes continuar la conversación actual con Copilot o iniciar otra seleccionando **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de pantalla de cómo seleccionar el archivo de instrucciones de la interfaz de usuario](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** sigue seleccionado en el menú desplegable de agentes de la vista Chat.

   ![Captura de pantalla del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad, como los helpers de la capa de datos, la interfaz de usuario y las pruebas. Después, modifica los archivos y ejecuta las pruebas.

   ![Captura de pantalla de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas: explorar el proyecto, modificar archivos y ejecutar pruebas. Según la complejidad de la tarea y del código base, puede tardar unos minutos. Durante el proceso quizá aparezcan los botones **Keep** y **Undo** en el editor. Cuando termine, tendrás la opción de conservar o deshacer todos los cambios; no es necesario seleccionarlos mientras el agente sigue trabajando.

7. Cuando Copilot te lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de pantalla de un cuadro de diálogo en el panel Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas e incluso que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y no detectar errores.

   ![Captura de pantalla de una sesión de Copilot Chat completa con el modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio en ejecución y explora la nueva funcionalidad.
11. Cuando hayas comprobado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has aprendido a usar el modo de agente de GitHub Copilot para añadir capacidades al sitio web de Tailspin Toys. Has aprendido:

- a implementar funciones en la interfaz de usuario y la capa de datos con el modo de agente de GitHub Copilot;
- a utilizar el modo de agente de Copilot para explorar el proyecto, identificar los archivos pertinentes y coordinar cambios;
- a revisar los cambios y las pruebas generados por el modo de agente de Copilot antes de combinarlos en el código base.

Ahora vamos a [probar la función con el servidor MCP de Playwright][next-lesson] y a abrir una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar la paginación

A medida que crezca la lista de juegos, será necesario habilitar la paginación. Aplica lo que has aprendido en este ejercicio y pide a Copilot que implemente la paginación. Ten en cuenta lo siguiente:

- sigue las prácticas recomendadas existentes, incluidos los archivos de instrucciones del proyecto;
- decide cómo quieres implementar la paginación: por ejemplo, si el usuario podrá seleccionar el tamaño de página o si estará fijado;
- al redactar la solicitud, proporciona a Copilot las indicaciones necesarias para que implemente la solución que quieres;
- quizá tengas que iterar con GitHub Copilot, solicitar cambios y proporcionar contexto. Este es el flujo habitual al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo se utilizan][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
