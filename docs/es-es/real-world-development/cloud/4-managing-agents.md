---
title: "Ejercicio 4: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores, has pedido al agente en la nube de Copilot que se encargue de tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente en la nube funciona de forma autónoma y asíncrona, es importante poder supervisar estas tareas.

Dispones de varias herramientas para gestionar las tareas asignadas al agente en la nube, incluida la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de los agentes que tienen solicitudes de incorporación de cambios abiertas. También puedes explorar las operaciones realizadas e incluso dirigir una sesión en curso.

En esta lección, aprenderás a:

- explorar la página de agentes para supervisar las tareas del agente en la nube;
- dirigir una sesión en curso para solicitar funcionalidades adicionales.

## Escenario

Tras asignar al agente la tarea de crear un modo de alto contraste, el equipo se ha dado cuenta de que también sería un buen momento para añadir un modo claro. Como ya se están actualizando los estilos del sitio y añadiendo la funcionalidad para cambiar de modo, parece razonable incluir esta función. Quieres dirigir el trabajo del agente para asegurarte de que añada el modo claro además del modo de alto contraste.

## Revisa las tareas del agente en la nube de Copilot

Vamos a consultar el estado de todas las tareas asignadas al agente en la nube de Copilot.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en los paneles principal e izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código base;
   - generar API para modificar juegos;
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso. Revisa las operaciones que ha realizado Copilot, que pueden incluir:
   - obtener el código del repositorio;
   - crear el entorno de trabajo de Copilot;
   - configurar servidores MCP;
   - realizar varios pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos exactos de la lista variarán según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Fíjate también en el panel de solicitudes de incorporación de cambios que aparece a la derecha. Desde allí puedes consultar la solicitud y los archivos modificados para seguir el trabajo.

## Dirige el agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión sobre la creación del modo de alto contraste. El título exacto dependerá del nombre que le haya dado Copilot y del estado actual del trabajo.

    ![Sesión de accesibilidad en el centro de control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado de configurar el entorno y ha empezado a trabajar. Lo sabrás cuando empiecen a aparecer mensajes similares a los que se muestran a continuación.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe esta solicitud:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de pantalla de la tarea del agente en la nube en la página de agentes, con el cuadro de diálogo Steer active session while Copilot is working resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar la solicitud.
5. Observa cómo Copilot la confirma y la incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud al flujo cuando termine el paso que estaba realizando en el momento en que enviaste el mensaje.

El proceso tardará varios minutos, así que puedes aprovechar para hacer una pausa y reflexionar sobre todo lo que has aprendido y explorado.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro principal para supervisar y dirigir las tareas del agente en la nube de GitHub Copilot. Desde este centro de control puedes seguir todas las tareas activas y completadas, revisar el trabajo que se está realizando e incluso redirigir tareas en curso para ajustar su alcance o proporcionar indicaciones adicionales.

Has aprendido:

- a utilizar el centro de control y la página de agentes para supervisar las tareas del agente en la nube;
- a redirigir una sesión en curso para solicitar funcionalidades adicionales.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
