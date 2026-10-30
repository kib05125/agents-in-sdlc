---
title: "Lección 2: Añade valoraciones con estrellas: una mejora rápida"
description: "Usa Copilot CLI para hacer un cambio pequeño en las tarjetas de juegos, revísalo en el navegador mediante el puerto reenviado y combínalo en tu primera solicitud de incorporación de cambios."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ahora que has instalado Copilot CLI y has probado una conversación, es el momento de hacer el primer cambio en el proyecto. Será pequeño: los datos de los juegos ya incluyen una valoración con estrellas, pero todavía no se muestra en las tarjetas de la página de inicio. Pedirás al agente que la muestre, revisarás el cambio y lo combinarás en tu primera solicitud de incorporación de cambios.

En esta lección, aprenderás a:

- iniciar una conversación específica de Copilot en una rama de funcionalidad;
- pedir al agente que haga un pequeño cambio en el proyecto;
- revisar el cambio con `/diff`;
- ejecutar la aplicación y comprobar el cambio en el navegador mediante el puerto reenviado;
- abrir y combinar tu primera solicitud de incorporación de cambios.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración con estrellas, que ya aparece en la página de detalles. Sin embargo, las tarjetas de la página de inicio solo muestran el título, la categoría, la editorial y la descripción. Como ejercicio inicial, pedirás al agente que muestre en cada tarjeta la valoración existente: es un cambio pequeño y autónomo, ideal para tu primera sesión.

## Anatomía de una conversación

Una **conversación** es el espacio donde trabajas con Copilot CLI en una tarea. A diferencia de la aplicación Copilot, una conversación normal de CLI utiliza el repositorio y la rama de Git que están seleccionados en el terminal, en vez de crear un árbol de trabajo específico. Las conversaciones guardadas permiten volver a la misma conversación más adelante, mientras que los archivos y la rama siguen formando parte del estado normal de Git en el disco.

En una conversación verás tres elementos: tus solicitudes y las respuestas del agente, la actividad de sus herramientas mientras explora y edita archivos, y los cambios que puedes inspeccionar con `/diff`.

## Inicia una conversación y solicita el cambio

Empecemos una conversación nueva para implementar la función.

1. Vuelve a tu Codespace.
2. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Si Copilot no está iniciado, ejecuta este comando:

   ```bash
   copilot --yolo
   ```

4. Inicia una sesión nueva con el comando slash `/new` y pulsa <kbd>Enter</kbd>.
5. Envía esta solicitud:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot explorará el proyecto, localizará los archivos que muestran los detalles de los juegos y creará el código necesario. ¡Ya has añadido una funcionalidad con Copilot CLI!

## Revisa el diff

Todos los cambios generados por IA deben revisarse antes de combinarlos, por pequeños que sean. Vamos a revisar los cambios directamente en Copilot CLI.

1. Escribe `/diff` e inspecciona todos los archivos modificados.
2. Comprueba que la tarjeta muestra la valoración numérica cuando existe y `No rating yet` cuando `starRating` es `null`.
3. Comprueba que las pruebas cubren ambos casos.
4. Revisa los resultados de las comprobaciones ejecutadas por Copilot y pídele que corrija los errores.
5. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la vista del diff.

> [!NOTE]
> Como Copilot, igual que todas las herramientas de IA generativa, es probabilístico y no determinista, el código exacto puede variar. Revisa el comportamiento en vez de esperar una implementación idéntica.

## Comprueba los cambios

No basta con leer el código y dar por hecho que funciona. Pide a Copilot que inicie el sitio web para poder examinar la interfaz de usuario (UI) actualizada en el navegador mediante el puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Confirma que las tarjetas muestran valoraciones sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor que inició:

   ```plaintext
   Stop the development server you started.
   ```

## Abre y combina tu primera solicitud de incorporación de cambios

¡Has creado la función! Ahora abrirás una solicitud de incorporación de cambios (PR) para combinar el nuevo código en el proyecto.

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Escribe `/pr create`. Copilot CLI publicará la confirmación existente al crear la PR y mostrará su URL.
3. Abre la PR manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la URL que aparece en Copilot CLI.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando esté todo listo, selecciona **Merge pull request** y confirma la combinación.
6. Vuelve al Codespace y sal de Copilot CLI con `/exit`.
7. Actualiza tu `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y siguientes pasos

¡Enhorabuena! Has publicado tu primer cambio con GitHub Copilot CLI. En concreto, has:

- iniciado una conversación de Copilot en una rama de funcionalidad;
- pedido al agente que hiciera un pequeño cambio en las tarjetas de juegos;
- revisado el cambio con `/diff`;
- ejecutado la aplicación y comprobado la valoración en el navegador mediante el puerto reenviado;
- abierto y combinado tu primera solicitud de incorporación de cambios.

A continuación, [partirás de la incidencia de filtrado y usarás los modos Plan y Autopilot][next-lesson] para crear una función más amplia.

## Recursos

- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
