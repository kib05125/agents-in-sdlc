---
title: "Ejercicio 5: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando utilizas el modo de agente de GitHub Copilot, este trabaja de forma autónoma: explora el código base, propone cambios, edita archivos y ejecuta comandos. Como trabaja en tu equipo en tiempo real, Copilot Chat en VS Code muestra cada llamada a herramientas y cada edición de archivo a medida que se producen. Puedes revisar cada diff en línea, aceptar o rechazar cambios concretos y orientar la conversación con solicitudes de seguimiento para refinar o ampliar el trabajo sin salir del editor.

En este ejercicio, aprenderás a:

- supervisar en tiempo real el trabajo del agente de accesibilidad;
- revisar y aceptar las ediciones propuestas en los archivos;
- dirigir al agente durante la sesión para que añada un selector de modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad que utilizaste en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha añadido una nueva solicitud: también quiere un selector de modo claro para quienes consideran que el tema predeterminado es demasiado oscuro. En vez de iniciar una conversación desde cero, orientarás la sesión actual para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición de archivo aparece en línea en el chat con un diff y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para observar cada llamada a herramienta. Verás lecturas y ediciones de archivos y los comandos de terminal que solicita ejecutar.
3. Cuando el agente proponga una edición, el diff aparecerá en línea:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para deshacerlo y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed**, en la parte superior del chat, para ver todos los archivos modificados por el agente durante la sesión.

> [!NOTE]
> Si una llamada a herramienta solicita permiso (por ejemplo, para ejecutar un comando de terminal), el agente se detendrá y mostrará una solicitud **Allow**. Aprueba únicamente los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Deberías ver un mensaje final que resuma los cambios realizados.

## Pide al agente que añada el modo claro

Ahora que ya está disponible el modo de alto contraste, ampliarás la misma conversación para añadir un selector de modo claro. Como continuarás en la misma sesión de chat, el agente conservará todo el contexto del trabajo anterior, incluidos los componentes modificados, el enfoque de estilos y el patrón de almacenamiento local.

1. En el campo de entrada de Copilot Chat, envía esta solicitud de seguimiento:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente continuará desde donde lo dejó, explorará los cambios realizados y propondrá los cambios necesarios para añadir el modo claro.
3. Revisa cada nueva edición a medida que aparezca, igual que hiciste con el modo de alto contraste. Usa **Keep** o **Undo** en cada diff.
4. Si el agente sigue un enfoque que no quieres (por ejemplo, intenta reorganizar la interfaz de los selectores de una forma que no te gusta), puedes detenerlo con el botón **Stop** junto al campo de entrada y enviar una solicitud de seguimiento para aclararlo.

> [!TIP]
> Dirigir una sesión en curso es más rápido que empezar de nuevo. Utiliza solicitudes de seguimiento para pedir ajustes («simplifica el CSS: utiliza variables CSS en vez de duplicar selectores»), corregir el enfoque («no añadas un tercer selector; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos selectores»).

## Revisa los cambios

Antes de confirmar el trabajo, revisa rápidamente todos los archivos que ha modificado el agente.

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver actualizaciones de componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos modificados y revisa los diffs. Comprueba que se apliquen los patrones de accesibilidad del agente personalizado: atributos ARIA, navegación con el teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has utilizado el modo de agente de Copilot Chat para supervisar y dirigir una sesión en curso: has revisado cada edición de archivo, la has aceptado o rechazado y has enviado solicitudes de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección has aprendido a:

- supervisar el trabajo de un agente en tiempo real;
- aceptar y rechazar ediciones individuales de archivos;
- dirigir una sesión en curso mediante solicitudes de seguimiento.

En la [siguiente lección][next-lesson], publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios que ha generado el agente personalizado local durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
