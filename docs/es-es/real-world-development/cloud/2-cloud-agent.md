---
title: "Ejercicio 2: Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que haya pocas organizaciones, si es que hay alguna, que no tengan dificultades con la deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidades que llevan tiempo en el backlog porque no había tiempo para implementarlas. El agente en la nube de GitHub Copilot está diseñado para realizar tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, genera un borrador de solicitud de incorporación de cambios para que lo revise una persona. Así puedes delegar tareas tediosas, acelerar el desarrollo y centrarte en cuestiones de mayor alcance.

En este ejercicio, explorarás con el agente en la nube de Copilot:

- cómo personalizar el entorno de generación de código;
- cómo garantizar que las operaciones se realicen de forma segura;
- por qué es importante definir incidencias con un alcance claro;
- cómo asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en un estado deficiente: comprobarás que no hay ninguna. Como primer paso, el equipo quiere añadir comentarios de documentación TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la forma de descubrir juegos. Quiere que la página de detalles de cada juego muestre juegos relacionados, es decir, otros títulos de la misma categoría, para que las personas puedan seguir explorando. Por ahora no necesita una implementación refinada; basta con algo que le permita probar la experiencia de usuario. Aunque esta tarea es un impedimento, ahora mismo hay otras incidencias de mayor prioridad.

Estas son tareas que suelen quedar relegadas y que resultan ideales para asignarlas al agente en la nube de Copilot. El agente puede trabajar en ellas de forma asíncrona mientras tú te ocupas de otras tareas y, después, puedes revisar su trabajo y comprobar que el resultado sea el esperado.

## Presentación del agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a un desarrollador. Al igual que cuando trabajas con otra persona, puedes asignarle tareas de varias formas, por ejemplo, [asignándole una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Una vez asignada, Copilot crea un borrador de solicitud de incorporación de cambios para hacer seguimiento del progreso, configura un entorno y empieza a trabajar. Puedes consultar la sesión de Copilot mientras está en curso o una vez finalizada. Cuando la solución propuesta esté lista para que la revises, te mencionará en la solicitud de incorporación de cambios.

## La importancia de definir bien las instrucciones

GitHub Copilot no hace magia: no puedes saltarte el análisis. Incluso las tareas que parecen sencillas pueden ser complejas cuando se examinan de cerca. Por eso, [define con cuidado el alcance de las tareas para el agente en la nube de Copilot][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. La IA generativa no cambia los fundamentos del desarrollo de software.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has conocido las instrucciones personalizadas y cómo orientan a Copilot. Si ya has completado un ejercicio práctico sobre instrucciones personalizadas, habrás visto cómo funciona `.github/copilot-instructions.md`; si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo al agente en la nube de Copilot, revisa los archivos de instrucciones que ya incluye este repositorio. Así podrás reconocer su efecto más adelante.

Abre los siguientes archivos en la interfaz web de GitHub o en un Codespace, si ya tienes uno en ejecución:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, en especial las expectativas sobre las convenciones de TypeScript y los comentarios de documentación TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en los metadatos `applyTo`, que limitan el alcance de estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation` en la sección siguiente, busca comentarios de documentación TSDoc, convenciones de TypeScript y encabezados de comentario. Estas directrices proceden de los archivos de instrucciones que acabas de revisar. Volveremos a mencionarlas al revisar las solicitudes de incorporación de cambios en un ejercicio posterior.

## Configura el entorno de desarrollo del agente en la nube de Copilot

Para crear código suele ser necesario configurar un entorno específico y ejecutar scripts de preparación. Lo mismo se aplica al asignar tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente en la nube utiliza [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo mediante un [flujo de trabajo de configuración especial][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece a trabajar. Así puede acceder a las herramientas y dependencias necesarias. Este flujo ya está configurado para facilitar el taller y permitirte aprender con la práctica. Asegura que Copilot tenga acceso a Node.js, las dependencias del proyecto, los navegadores para las pruebas de extremo a extremo y la base de datos SQLite migrada y precargada para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Define los permisos mínimos necesarios para *tus pasos*. Copilot tendrá su propio token para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de los pasos de configuración, por ejemplo, para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas en estos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración del frontend y la aplicación: Node.js (ahora toda la aplicación utiliza Astro + Drizzle/libSQL)
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

      # Migra y precarga la base de datos SQLite local para que las compilaciones y pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Incluye un único trabajo llamado `copilot-setup-steps`. Se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- Observa el desencadenador `workflow_dispatch`, que permite ejecutar el flujo manualmente desde la pestaña **Actions** del repositorio. Es útil para comprobar que funciona correctamente, en vez de esperar a que Copilot lo ejecute.

## Añade documentación

Todo el mundo entiende la importancia de la documentación, pero muchos proyectos tienen información desactualizada o carecen de ella. Esta deuda técnica suele quedar desatendida, lo que ralentiza el trabajo y dificulta mantener el código base o incorporar a nuevos desarrolladores. Copilot destaca en la generación de documentación, así que esta incidencia es ideal para asignarla al agente en la nube. Trabajará en segundo plano y generará la documentación necesaria. En un ejercicio posterior volverás a revisar su trabajo.

1. Abre una nueva pestaña del navegador y ve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de una incidencia nueva.
4. Selecciona **Blank issue** para crearla.
5. Escribe `Code lacks documentation` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de incorporación de cambios (PR) que se acaba de generar, cuyo título será similar a `[WIP]: Code lacks documentation`. Si aún no aparece en la lista, espera un momento y actualiza la ventana del navegador.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista de tareas de Copilot puede tardar varios minutos en aparecer en la solicitud de incorporación de cambios. Copilot está creando su entorno (ejecutando el flujo de trabajo anterior), analizando el proyecto y determinando el mejor enfoque para la tarea.

13. Revisa la lista y las tareas que va a completar.
14. Desplázate por la cronología de la solicitud de incorporación de cambios. Deberías ver una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Es posible que tengas que actualizar la ventana para ver el indicador actualizado.

16. Observa que puedes recorrer la sesión en directo y ver cómo Copilot resuelve el problema: explora el código y comprende su estado, se detiene a analizar y decidir cuál es el plan adecuado y crea código.

El proceso tardará varios minutos. Una de las principales ventajas del agente en la nube de Copilot es que puede trabajar de forma asíncrona y permitirte centrarte en otras tareas. Aprovecharemos esa función: asignaremos otra tarea al agente y, mientras tanto, nos ocuparemos de añadir funcionalidades a la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Una de las grandes ventajas del agente en la nube de GitHub Copilot es que permite dividir el trabajo: tú puedes centrarte en unas tareas mientras el agente se ocupa de otras. Aunque añadir una sección de juegos relacionados para el equipo de diseño quizá no lleve mucho tiempo, puedes aprovechar ese tiempo para otras tareas. ¡Vamos a asignársela al agente en la nube!

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de una incidencia nueva.
4. Selecciona **Blank issue** para usar la plantilla en blanco.
5. Escribe `Show related games on the game details page` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, aparecerán unos 👀 en el primer comentario de la incidencia para indicar que Copilot ya está trabajando.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda solicitud. El agente en la nube funciona de manera parecida a un ingeniero de software: no necesitas supervisarlo continuamente, puedes revisar el resultado cuando termine. Ahora vamos a crear y utilizar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignarle incidencias para que las resuelva de forma asíncrona. Copilot puede ayudarte con deuda técnica, crear funcionalidades o migrar código de un framework a otro.

Has aprendido:

- a personalizar el entorno de generación de código;
- a garantizar que las operaciones se realicen de forma segura;
- por qué es importante definir incidencias con un alcance claro;
- a asignar incidencias a Copilot.

Mientras el agente en la nube trabaja, puedes crear y utilizar agentes personalizados. [El agente en la nube de Copilot también puede utilizar servidores MCP][cloud-agent-mcp] y acceder a las instrucciones personalizadas que has explorado en módulos anteriores.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de trabajo de configuración del agente en la nube de Copilot][cloud-agent-best-practices]

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
