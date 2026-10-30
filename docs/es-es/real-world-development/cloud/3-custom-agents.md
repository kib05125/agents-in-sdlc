---
title: "Ejercicio 3: Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para tareas o ámbitos concretos de tu flujo de desarrollo. Al definir agentes en archivos Markdown dentro de la carpeta `.github/agents` del repositorio, puedes proporcionar a Copilot instrucciones específicas, prácticas recomendadas, patrones de programación y conocimientos del dominio para que realice determinados tipos de trabajo de forma más eficaz. Los equipos pueden plasmar su experiencia en agentes reutilizables: por ejemplo, un agente de accesibilidad que aplique las directrices de [WCAG][wcag], un agente de seguridad que siga prácticas de programación segura o un agente de pruebas que mantenga patrones de pruebas coherentes.

Los agentes personalizados se definen en archivos Markdown de la carpeta `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo tiene metadatos YAML con, como mínimo, los campos `name` y `description`, seguidos de una instrucción en Markdown que define el comportamiento, la experiencia y las indicaciones del agente.

### Diferencias entre los agentes personalizados y las habilidades de agente

Hay cierto solapamiento entre los agentes personalizados y las [habilidades de agente][agent-skills-concept]. Ambos se definen principalmente mediante archivos Markdown e indican a una IA cómo realizar operaciones. La forma más sencilla de distinguirlos es esta: un **agente personalizado** es el trabajador y las **habilidades** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y están diseñados para coordinar habilidades (e incluso otros agentes) durante el trabajo. En este taller, el agente personalizado de accesibilidad revisará y actualizará el sitio según las directrices de accesibilidad; para hacerlo, podría utilizar habilidades como una para el flujo de trabajo de solicitudes de incorporación de cambios o una para ejecutar y gestionar pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Como ocurre con cualquier elemento de IA, prueba distintas opciones e itera para encontrar la que mejor funcione en tus entornos y escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Con los agentes personalizados, explorarás:

- cómo se definen los agentes personalizados;
- cómo asignar una tarea a un agente personalizado.

## Escenario

Tailspin Toys se compromete a que todas las personas puedan utilizar su plataforma de financiación colectiva, independientemente de sus capacidades o preferencias visuales. Los comentarios recientes han indicado que algunas personas tienen dificultades para leer el tema oscuro actual debido al contraste insuficiente entre el texto y el fondo. Para solucionar este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

Como la accesibilidad es fundamental, quieres implementar esta función cuanto antes. Para generarla, utilizarás un agente personalizado.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Vamos a revisar su contenido para entender cómo orientará a Copilot.

Vuelve a tu Codespace y revisa el archivo del agente personalizado de accesibilidad:

1. Abre `.github/agents/accessibility.md`.
2. Observa los campos `name` y `description` de los metadatos YAML.

> [!CAUTION]
> Los campos `name` y `description` de los metadatos son obligatorios para los agentes personalizados.

3. Lee las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible;
   - las prácticas recomendadas de accesibilidad;
   - ejemplos de código HTML, CSS y JavaScript;
   - problemas y errores habituales.

## Crea y asigna una incidencia

El centro de control es el lugar principal para trabajar con todos los agentes de tu entorno. Desde allí, puedes asignar tareas al agente en la nube de Copilot, supervisarlas y redirigirlas o proporcionar indicaciones adicionales. Vamos a asignar a Copilot la tarea de crear el modo de alto contraste.

1. Ve a tu repositorio.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de una incidencia nueva.
4. Selecciona **Blank issue** para crear una incidencia en blanco.
5. Escribe `Add high contrast mode to website` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.
9. Selecciona **Accessibility agent** en la lista de agentes personalizados.

    ![Captura de pantalla de la asignación del agente en la nube, con el agente personalizado de accesibilidad resaltado](../../../_images/ex5-select-custom-agent.png)

10. Selecciona **Assign**.
11. ¡Copilot empezará a trabajar en segundo plano!

## Resumen y siguientes pasos

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes de IA especializados en tareas y ámbitos concretos. Los agentes personalizados te permiten convertir la experiencia y los estándares del equipo en agentes reutilizables que orientan a Copilot para que realice determinados tipos de trabajo de forma más eficaz.

Has aprendido:

- cómo se definen los agentes personalizados;
- cómo asignar una tarea a un agente personalizado.

Mientras Copilot trabaja en el modo de alto contraste, puedes [supervisar y dirigir la sesión del agente][next-lesson] desde el centro de control.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Preparar la organización para usar agentes personalizados][org-custom-agents]
- [Preparar la empresa para usar agentes personalizados][enterprise-custom-agents]

---

| [← Lección anterior: Agente en la nube de GitHub Copilot][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
