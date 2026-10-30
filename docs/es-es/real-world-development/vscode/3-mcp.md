---
title: "Ejercicio 3: Prueba tu función con el servidor MCP de Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo de agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear la función de filtrado con el modo de agente. Antes de abrir una solicitud de incorporación de cambios, debes comprobar que funciona en el navegador. En vez de probar la aplicación manualmente, conectarás el **servidor MCP de Playwright** y pedirás a Copilot que controle un navegador real para probar la función. Después publicarás la rama y abrirás la solicitud de incorporación de cambios.

En este ejercicio, aprenderás a:

- qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían Copilot con nuevas herramientas;
- añadir el servidor MCP de Playwright al espacio de trabajo;
- pedir a Copilot que lo utilice para probar manualmente la función de filtrado en un navegador;
- publicar la rama y abrir una solicitud de incorporación de cambios para el trabajo de filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo de agente es mucho más potente cuando puede acceder a recursos externos al editor. Copilot lo consigue mediante Model Context Protocol (MCP), un estándar que permite al agente comunicarse con herramientas y servicios externos.

![Diagrama del funcionamiento interno del modo de agente y su interacción con el contexto, el LLM y las herramientas, incluidas las aportadas por servidores MCP y extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Se accede a estas herramientas y recursos a través de un servidor MCP, que actúa como puente entre el agente de IA y los servicios externos. El servidor MCP gestiona la comunicación entre el agente y las herramientas externas, como API existentes o herramientas locales (por ejemplo, paquetes NPM). Cada servidor MCP representa un conjunto distinto de herramientas y recursos a los que puede acceder el agente.

Algunos servidores MCP populares:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona acceso a un conjunto de API para gestionar repositorios de GitHub. Permite que el agente realice acciones como crear repositorios, actualizar los existentes y gestionar incidencias y solicitudes de incorporación de cambios.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: proporciona funciones de automatización del navegador mediante Playwright. Permite que el agente realice acciones como navegar por páginas web, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP que proporcionan acceso a distintas herramientas y recursos. GitHub aloja un [registro MCP](https://github.com/mcp) para facilitar la búsqueda y las contribuciones al ecosistema.

> [!CAUTION]
> Por motivos de seguridad, trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de utilizar uno, revisa detenidamente su código fuente, verifica quién lo publica y considera las implicaciones de seguridad. Utiliza únicamente servidores MCP de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisa la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en tu Codespace.
2. Deberías ver que ya hay un servidor `github` configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

La plantilla del proyecto ya incluye esta entrada `github`, por eso Copilot pudo leer el backlog de incidencias en el ejercicio anterior. Utiliza el [servidor MCP remoto de GitHub][remote-github-mcp-server], así que no hay que instalar nada localmente: VS Code se conecta mediante HTTP y te autenticas con GitHub la primera vez que Copilot utiliza una de sus herramientas.

## Añade el servidor MCP de Playwright

Ahora añadirás un segundo servidor. El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador que puede controlar, justo lo que necesitas para probar la función.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github` para que el archivo quede así:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es un servidor **local**: VS Code lo inicia en el equipo ejecutando `npx @playwright/mcp@latest`. La marca `--headless` indica a Playwright que ejecute el navegador sin mostrar una ventana, algo necesario en un Codespace, que no dispone de un escritorio para mostrarla.

> [!NOTE]
> El proyecto Tailspin Toys ya utiliza Playwright para las pruebas de extremo a extremo, por lo que normalmente el navegador que necesita Playwright ya está instalado. Si Copilot indica más adelante que falta un navegador, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

## Inicia y autoriza el servidor

VS Code inicia los servidores MCP **cuando se necesitan**: la primera vez que Copilot requiere las herramientas de un servidor, VS Code lo inicia y te pregunta si confías en él.

1. En `.vscode/mcp.json`, aparecerá una acción **Start** encima de la entrada `playwright`. Selecciónala para iniciar el servidor y comprobar que la configuración es correcta.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y confirma que confías en él para que pueda iniciarse.

Iniciarlo manualmente es opcional. Si lo omites, VS Code iniciará el servidor y te pedirá que confirmes que confías en él la primera vez que Copilot necesite sus herramientas en el paso siguiente.

## Prueba la función de filtrado

El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador real que puede controlar. En vez de probar la aplicación manualmente, el agente puede abrir una página, navegar por ella, aplicar filtros y leer los resultados. Después, puede resumirte lo que ha observado. Es una forma rápida de comprobar que una función se comporta como esperas sin salir de la conversación.

Internamente, el servidor MCP de Playwright utiliza el [árbol de accesibilidad][playwright-mcp-server] de la página en lugar de capturas de pantalla. El agente razona sobre elementos estructurados y etiquetados (botones, enlaces y elementos de lista), igual que las tecnologías de asistencia. Por eso, una comprobación funcional rápida también sirve como una revisión básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe la función de filtrado:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot abrirá un navegador mediante el servidor MCP de Playwright, seguirá los pasos y te comunicará lo que encuentre. Compara el resumen con los criterios de aceptación de la incidencia. Si algo no parece correcto, haz preguntas de seguimiento o pídele que corrija el código antes de abrir una solicitud de incorporación de cambios.

> [!NOTE]
> La aplicación debe estar en ejecución en `http://localhost:4321` para realizar esta prueba. Si has detenido el servidor de desarrollo, vuelve a iniciarlo antes de enviar la solicitud. La primera vez que Copilot utiliza el servidor MCP de Playwright, quizá tenga que descargar un navegador. Si indica que falta uno, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publica la rama y crea una solicitud de incorporación de cambios

Ahora que has comprobado que la función funciona, puedes abrir una solicitud de incorporación de cambios (PR) para que el equipo la revise. El primer paso es publicar la rama `filtering-vscode`.

1. Abre el panel **Source Control** del Codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Captura de pantalla del panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crea la solicitud de incorporación de cambios

Puedes crear una solicitud de incorporación de cambios de varias formas, por ejemplo, en github.com o mediante la interfaz de línea de comandos (CLI) de GitHub. Como ya estás trabajando con GitHub Copilot, pídele que la cree. Puede buscar la incidencia pertinente y asociarla a la solicitud.

1. Ve al panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Comprueba que **Agent** está seleccionado en el menú desplegable de agentes para que Copilot pueda utilizar las herramientas de GitHub.
3. Pide a Copilot que cree una PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Cuando sea necesario, selecciona **Continue** para permitir que Copilot reúna información y realice las operaciones. La primera vez que utilice una herramienta de GitHub, quizá debas autenticarte. Sigue las instrucciones para permitirlo.
5. Observa cómo Copilot busca entre las incidencias, encuentra la correcta y crea la solicitud.
6. Selecciona el enlace que genera Copilot para revisar la solicitud, pero **no la combines todavía**.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has:

- aprendido qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían Copilot con nuevas herramientas;
- añadido el servidor MCP de Playwright al espacio de trabajo;
- utilizado el servidor para probar manualmente la función de filtrado en un navegador antes de publicarla;
- publicado la rama y abierto una solicitud de incorporación de cambios para el trabajo de filtrado.

Has utilizado un servidor MCP para *probar* una función, pero MCP es solo una forma de ampliar Copilot. A continuación, vamos a [crear un agente personalizado][next-lesson] para agilizar tareas específicas, como las revisiones de accesibilidad.

## Recursos

- [Qué es MCP y por qué habla todo el mundo de ello][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
