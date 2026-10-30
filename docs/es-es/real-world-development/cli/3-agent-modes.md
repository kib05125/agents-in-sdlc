---
title: "Lección 3: Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear los filtros a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos por añadir una función pequeña al proyecto, pero los cambios de mayor alcance requieren un proceso más sólido. GitHub Copilot CLI está diseñado para integrarse en el flujo de trabajo de una organización y ayudarnos a crear lo adecuado de la forma correcta. Esta es la primera de varias lecciones en las que seguirás un proceso habitual de desarrollo dirigido por agentes: partirás de una incidencia para crear una función, comprobarás que el código es válido y que la función se comporta como esperas y, por último, la combinarás en el proyecto.

> [!NOTE]
> A medida que avances en el flujo de trabajo de la función, utilizarás la misma conversación y rama. Normalmente se usarían ramas o PR distintas para los diferentes tipos de archivo, pero aquí simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección, aprenderás a:

- iniciar una conversación de Copilot desde una incidencia de GitHub;
- definir requisitos en el modo Plan;
- implementar la función con el modo Autopilot;
- revisar el código;
- validar manualmente la función en el navegador mediante el puerto reenviado.

Mientras sigues trabajando en esta función, actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks`, añadirás validación con MCP, crearás un agente de QA y abrirás la PR de la función.

## Escenario

El catálogo de Tailspin Toys está creciendo y quienes lo visitan necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la función, pero antes de programarla hay que acordar algunos detalles, como la combinación de categorías. Utilizarás el modo Plan para tomar esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

La incorporación de agentes de programación con IA al flujo de desarrollo no cambia los fundamentos. Si acaso, los hace aún más importantes. El flujo de la mayoría de los desarrolladores es parecido a este:

1. Partir de una incidencia registrada que describa el trabajo.
2. Crear un plan de lo que se debe desarrollar.
3. Crear y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Cuando se haya revisado el código y se hayan superado las comprobaciones de integración continua, combinarlo.

> [!NOTE]
> Los detalles exactos variarán según el equipo y la organización, pero la mayoría de los flujos de trabajo siguen un enfoque parecido.

Al seguir este proceso habitual, te aseguras de que el código generado por IA cumpla los requisitos y pase por el mismo proceso de validación que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla la autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: trabajas con el agente. Propone cambios y espera tu respuesta antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en el modo Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Parte de la incidencia

Antes de empezar a trabajar en los filtros, vuelve al Codespace y comprueba que el repositorio y el terminal estén preparados.

1. Vuelve al Codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que la PR de las valoraciones con estrellas se ha combinado.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para trabajar en los filtros:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de flecha para resaltar la incidencia **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia a la solicitud y volver a la pestaña **Session**.

Observa que la solicitud ahora empieza por `#7` (o un número parecido). El carácter `#` permite añadir al contexto una incidencia o una solicitud de incorporación de cambios de GitHub.

## Planifica la función de filtrado

La planificación te permite definir cómo se implementará una función o se realizará una tarea antes de delegársela a Copilot. Siempre es buena idea dedicar tiempo a planificar las tareas complejas. Cambia al modo Plan y pide a Copilot que cree el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Comprueba que el indicador debajo de la solicitud muestre **Plan**.
2. Después de la referencia a la incidencia que añadiste en el paso anterior, escribe:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot empezará a desarrollar el plan. Primero explorará el proyecto y, después, determinará el mejor enfoque.

3. Es posible que Copilot te pregunte cómo deberían funcionar los filtros. Responde según tus preferencias; no hay respuestas incorrectas.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para ampliarlo.
5. Desplázate para revisar el plan.
6. Pide a Copilot que revise cualquier parte que no coincida con tus decisiones.

## Autoriza Autopilot

Una vez redactado y revisado el plan, es el momento de implementarlo. Deja que Copilot se encargue del trabajo con Autopilot.

Autopilot permite que Copilot itere sobre el problema hasta que considere que lo ha resuelto.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente en la versión instalada.
2. Comprueba que el indicador debajo de la solicitud muestre **Autopilot**.
3. Observa cómo Copilot sigue el plan, genera código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación de inmediato, así que revisa primero el plan. Si Copilot indica que faltan dependencias o que hay un conflicto de puertos, resuelve el problema de configuración antes de dar por terminadas las comprobaciones.

## Revisa y verifica la implementación

Debes revisar el código generado antes de combinarlo, como harías con cualquier otro código. Vamos a revisar el código y ejecutar el sitio para comprobar que todo funciona.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para volver al modo Interactive. Comprueba que el indicador ya no muestre **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación de los filtros y sus pruebas.
3. Compara el resultado con la incidencia y las decisiones tomadas durante la planificación.
4. Cuando hayas revisado el código, pulsa <kbd>Esc</kbd> para salir de la vista del diff.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que resuelva cualquier error.

## Explora la nueva funcionalidad

El código parece correcto, pero ¿funciona? Vamos a iniciar la aplicación como antes y abrir el sitio mediante el puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros de categoría y editorial y las combinaciones acordadas en el plan.
4. Comprueba que el restablecimiento y los resultados vacíos coincidan con la incidencia y tus decisiones.
5. Vuelve al Codespace y pide a Copilot que detenga el servidor de desarrollo que inició.

## Resumen y siguientes pasos

Has utilizado distintos modos de conversación para crear y revisar una función. En esta lección has:

- iniciado una conversación de Copilot desde una incidencia de GitHub;
- definido los requisitos en el modo Plan;
- implementado la función con el modo Autopilot;
- revisado el código;
- validado manualmente la función en el navegador mediante el puerto reenviado.

A continuación, profundizarás en cómo se genera el código y cómo garantizar que siga las prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
