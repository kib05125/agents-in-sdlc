---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

En este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le has pedido que añada documentación al código, cree una función de juegos relacionados para que el equipo de diseño pueda iterar y añada funciones de accesibilidad, incluidos los modos de alto contraste y claro. Vamos a revisar los cambios de código y, si hace falta, proporcionar comentarios a Copilot para mejorar su trabajo.

### Escenario

Como ya hemos indicado varias veces, los fundamentos del diseño de software y DevOps no cambian con la llegada de la IA generativa. Siempre debemos revisar el código generado y seguir nuestro proceso habitual de DevOps. Con esto en mente, revisemos las sugerencias de GitHub Copilot para crear la documentación, la función de juegos relacionados y las funciones de accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y agente en la nube de GitHub Copilot

Como el agente en la nube de Copilot realiza tareas de forma asíncrona y sin supervisión, se han establecido ciertas restricciones de seguridad para garantizar que todo siga siendo seguro. Entre ellas:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utilizará para su código.
- El agente en la nube se ejecuta en GitHub Actions, donde crea un entorno independiente y efímero para trabajar.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de que puedan ejecutarse.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empecemos por la primera solicitud de incorporación de cambios (PR) generada por el agente en la nube de GitHub Copilot: la que añade documentación al código. Para esta tarea, utilizarás la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Al explorar la PR, es posible que veas una advertencia que indica que el firewall bloqueó a GitHub Copilot. Es **lo esperado**, ya que Copilot tiene acceso limitado a recursos externos de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube de Copilot][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes de incorporación de cambios.
3. Abre la solicitud cuyo título sea parecido a **Add missing documentation** o que incluya una descripción más completa.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la solicitud de incorporación de cambios incluirá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos; mientras tanto, puedes descansar o reflexionar sobre lo que has aprendido.

4. Cuando la solicitud esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye los nuevos comentarios de documentación TSDoc y otra documentación. Los cambios exactos variarán.

    Al revisar los cambios, busca comentarios de documentación TSDoc, convenciones de TypeScript y encabezados de comentario. Proceden de los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.

6. Cuando hayas revisado las actualizaciones y te parezcan correctas, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que informa de que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, verás que las comprobaciones del proyecto para la aplicación Astro pasan. El proceso puede tardar unos minutos.

## Pide cambios a GitHub Copilot

La colaboración con Copilot en una solicitud de incorporación de cambios no es un proceso unidireccional. Puedes mencionarlo en comentarios de la solicitud, igual que harías con otra persona del equipo, o añadir comentarios en línea al código. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no podemos darte una instrucción exacta. Algunas ideas de cambios que puedes solicitar:

- Añadir encabezados de comentario al principio de cada archivo de código con una breve descripción de su función.
- Añadir comentarios de documentación TSDoc a los archivos TypeScript y Astro.
- Crear un archivo README que describa la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier otra persona. Puedes usar una de las ideas anteriores o sugerir otro cambio relacionado con la documentación del código.
2. Selecciona **View Session** para observar cómo Copilot trabaja. Verás que inicia una nueva sesión para realizar los cambios.
3. Puedes seleccionar **Back to pull request** para volver a la solicitud.

    ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, debería aparecer una nueva confirmación en la solicitud.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Cuando lo estés, puedes marcar la PR como lista para revisión y combinarla en la rama principal.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la función de juegos relacionados

Volvamos a la PR que Copilot generó para resolver la incidencia sobre la sección de juegos relacionados en la página de detalles.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la solicitud cuyo título sea parecido a **Show related games on the game details page** o que incluya una descripción más completa.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado las actualizaciones y te parezcan correctas, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador que informa de que hay flujos de trabajo pendientes de aprobación.
7. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, verás que las comprobaciones del proyecto para la aplicación Astro pasan. El proceso puede tardar unos minutos.
9. **Opcional:** también puedes cambiar a esta rama en tu Codespace para probar manualmente la función de juegos relacionados. Ve al Codespace, abre el terminal y ejecuta los siguientes comandos (sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

¡Copilot ha creado la función de juegos relacionados! Como antes, puedes iterar con el agente en la nube y pedirle que haga cambios. Por ejemplo, podrías pedirle que ajuste el número de juegos relacionados o que añada encabezados de comentario y comentarios de documentación TSDoc (recuerda que asignaste esta tarea **antes** de actualizar las instrucciones personalizadas). Para ello, añade un comentario nuevo en la pestaña **Conversation**; Copilot lo verá e iniciará otra sesión.

## Revisa las funciones de accesibilidad

Por último, revisaremos las funciones de accesibilidad implementadas con el agente personalizado de accesibilidad. Esta PR debe incluir tanto el modo de alto contraste asignado en el ejercicio 3 como el modo claro solicitado en el centro de control durante el ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la solicitud cuyo título sea parecido a **Add high contrast mode to website** o que incluya una descripción más completa.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la solicitud de incorporación de cambios incluirá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta especial atención a:
   - los componentes de interfaz para alternar entre modos;
   - el uso del almacenamiento local para guardar las preferencias;
   - los cambios de CSS o de estilos para los modos de alto contraste y claro;
   - los atributos de accesibilidad (etiquetas ARIA, navegación con el teclado, etc.);
   - el código JavaScript/TypeScript que gestiona el cambio de modo.

6. Cuando hayas revisado las actualizaciones y te parezcan correctas, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que informa de que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, verás que las comprobaciones del proyecto para la aplicación Astro pasan. El proceso puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en tu Codespace para probar manualmente las funciones de accesibilidad. Ve al Codespace, abre el terminal y ejecuta los siguientes comandos (sustituye `<branch-name>` por el nombre de la rama creada por Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    A continuación, inicia la aplicación y prueba los modos de alto contraste y claro en el navegador. Comprueba que funcionan como esperas y que las preferencias se conservan al recargar la página.

Observa cómo el agente personalizado de accesibilidad ha orientado a Copilot para implementar estas funciones siguiendo las prácticas recomendadas de accesibilidad. Si encuentras algún problema o una posible mejora, puedes mencionarlo en un comentario a **@copilot**, como hiciste con las solicitudes anteriores.

## Ejercicio opcional: sigue delegando

El agente en la nube funciona mejor cuando puedes delegarle elementos reales del backlog y ocuparte de otras tareas. Para crear ese hábito, registra algunas incidencias más en tu repositorio y asígnaselas a Copilot. Por ejemplo:

- Crear un formulario para indicar el interés de los patrocinadores en la página de detalles de un juego.
- Implementar la paginación en la página de la lista de juegos.
- Añadir validación de entradas y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el entorno del agente en la nube. En estas lecciones:

- **Has revisado los archivos de instrucciones personalizadas incluidos en el repositorio** para reconocer después su efecto en el resultado del agente.
- **Has asignado incidencias al agente en la nube de Copilot** y has observado cómo prepara el entorno, planifica y ejecuta el trabajo de forma asíncrona.
- **Has creado y utilizado un agente personalizado** para accesibilidad y has añadido modos de alto contraste y claro.
- **Has usado la página de agentes de Copilot como centro de control** para supervisar y dirigir la sesión de accesibilidad mientras estaba en curso.
- **Has revisado las solicitudes de incorporación de cambios del agente en la nube e iterado sobre ellas**, mencionando a `@copilot` para solicitar cambios y aprobando flujos de trabajo.

## Revisión y siguientes pasos

Has completado el entorno del agente en la nube. Si quieres seguir explorando, los otros entornos complementan lo que has practicado:

- 🖥️ **[Entorno de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración con MCP directamente desde el IDE.
- 💻 **[Entorno de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y el puente opcional `/delegate` con el agente en la nube.

En tu propio repositorio, prueba estas ideas:

- Asigna al agente en la nube una incidencia de refactorización o cobertura de pruebas.
- Crea un agente personalizado para otro ámbito.
- Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de trabajo de configuración del agente en la nube de Copilot][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube de Copilot][agent-firewall]

---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
