---
title: "Lección 9: Comandos slash de GitHub Copilot CLI"
description: "Explora los comandos slash para gestionar el contexto, seleccionar modelos, compartir sesiones y delegar tareas opcionalmente a la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier buena herramienta de CLI, GitHub Copilot CLI incluye muchos comandos slash. Estos comandos permiten acceder a funciones avanzadas, información interna y opciones de configuración adicionales. Ya has utilizado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Vamos a explorar otros que pueden resultar útiles.

En esta lección, aprenderás a:

- utilizar `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación;
- utilizar `/model` para explorar los modelos disponibles;
- aprender cómo `/share` puede exportar o compartir una sesión;
- explorar comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas al agente en la nube.

## Escenario

Has completado el flujo de trabajo principal de CLI. Ahora explorarás algunas funciones adicionales: gestionar el contexto, cambiar de modelo, compartir sesiones y delegar trabajo opcionalmente al [agente en la nube de Copilot][about-cloud-agent].

## Explora el contexto de Copilot CLI

Al trabajar en tareas grandes o complejas, puede que alcances el límite de contexto del modelo. Copilot CLI compacta automáticamente la conversación cuando es necesario, y puedes inspeccionar o compactar el contexto tú mismo con comandos slash.

1. Vuelve al Codespace e inicia Copilot CLI desde la raíz del repositorio si aún no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y la distribución del contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio disponible.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a escribir `/context` y compara el resultado. Puede que no haya cambiado mucho si la conversación ya era pequeña.

> [!NOTE]
> Copilot CLI compacta automáticamente el contexto a medida que se llena la ventana. Utiliza `/compact` para elegir cuándo hacerlo. Usa `/clear` o `/new` cuando cambies a una tarea no relacionada y quieras empezar una conversación nueva.

## Elige el modelo

Cada modelo tiene puntos fuertes distintos y cada desarrollador tiene preferencias diferentes. Copilot CLI permite mostrar la lista y seleccionar el modelo que quieres utilizar.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información sobre su uso.
3. Mantén el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Comparte una sesión

Colaborar y compartir lo aprendido ayuda a todo el equipo a mejorar el uso de las herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartir o publicar un gist de GitHub.

1. Escribe `/help` y revisa las opciones de `/share` disponibles en la versión instalada.
2. Si quieres compartir la sesión, elige el destino adecuado, por ejemplo, `/share file` para exportar un archivo Markdown local.
3. Revisa el contenido exportado antes de enviarlo o publicarlo. Las exportaciones pueden incluir solicitudes, respuestas y detalles del proyecto.

Publicar un enlace o un gist es opcional. No publiques contenido del repositorio ni de la conversación que el equipo no tenga previsto compartir.

## Opcional: amplía el trabajo o delégalo

Has completado el taller principal. Copilot CLI también incluye comandos para tareas más amplias:

- `/fleet` puede dividir una tarea en subtareas independientes, asignarlas a subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un árbol de trabajo de Git aislado para una tarea distinta.
- `/delegate` puede enviar una tarea al agente en la nube de Copilot, que trabaja de forma asíncrona y puede abrir una solicitud de incorporación de cambios.

Estos comandos son opcionales porque pueden crear árboles de trabajo adicionales o trabajo remoto. Antes de probarlos, inicia una tarea nueva con un alcance claro y revisa el resultado mediante el flujo de trabajo habitual. Si quieres profundizar en el trabajo asíncrono de los agentes, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y siguientes pasos

Los comandos slash de Copilot CLI permiten configurar la herramienta, compartir sesiones y consultar qué ocurre en segundo plano. En esta lección has:

- utilizado `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación;
- utilizado `/model` para explorar los modelos disponibles;
- aprendido cómo `/share` puede exportar o compartir una sesión;
- explorado comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas al agente en la nube.

Hay más comandos slash y funciones de Copilot CLI por explorar. Vamos a terminar el recorrido [revisando lo aprendido][next-lesson] y algunos pasos para seguir aprendiendo.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca del agente en la nube de Copilot][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
