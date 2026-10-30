---
title: "Lección 10: Resumen y siguientes pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI durante todo el flujo de trabajo de Tailspin Toys. Has:

- preparado un Codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia de filtrado precargada;
- añadido valoraciones con estrellas, revisado el resultado en el navegador mediante el puerto reenviado y combinado manualmente tu primera solicitud de incorporación de cambios (PR);
- partido de la incidencia de filtrado, definido el enfoque en modo Plan, implementado la función con Autopilot y revisado el resultado en modo Interactive;
- guiado al agente con instrucciones personalizadas y, después, personalizado la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos;
- añadido el servidor Model Context Protocol (MCP) de Playwright y utilizado el navegador para explorar los filtros;
- creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas en el navegador;
- revisado todo el trabajo de filtrado y autorizado Agent Merge para combinar la PR de la función;
- explorado comandos slash para gestionar el contexto, los modelos, las sesiones compartidas y la delegación opcional a la nube.

## Lo que has publicado

El taller incluye dos hitos de PR, cada uno en su propia rama basada en `main` actualizado:

1. **Valoraciones con estrellas**: muestra el valor existente de `starRating` y un estado explícito para los juegos sin valorar.
2. **Filtros y flujo de trabajo de calidad**: implementa los filtros, actualiza las instrucciones y las aplica a la función, personaliza el informe de `quality-checks`, crea un perfil de QA e incluye las pruebas correspondientes.

Desde la planificación de los filtros hasta la apertura de su PR, has utilizado la misma conversación y rama. Hemos combinado ese trabajo en una sola PR para agilizar el taller.

## Diferentes tipos de verificación

Has comprobado el código de varias formas: pruebas automatizadas, inspección manual en el navegador y exploración del navegador por parte de Copilot mediante MCP. La habilidad `quality-checks` ejecutó las pruebas unitarias, el lint y las comprobaciones de tipos, y presentó los resultados con el nuevo formato. QA combinó esos resultados con una revisión de los requisitos y la cobertura de pruebas antes de la PR.

Las pruebas nuevas deben cubrir carencias reales; si QA no necesita añadir ninguna, el resultado puede ser correcto. Antes de autorizar una combinación, revisa el código y las pruebas y actualiza las comprobaciones afectadas si se producen cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller has actualizado instrucciones, personalizado una habilidad, creado un perfil de QA y configurado un servidor MCP. Reutiliza estas personalizaciones en distintas conversaciones y ajústalas a las necesidades del equipo. Las instrucciones definen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas, no solo el resumen del agente.

Elige el **modo y el modelo** adecuados para la tarea. Usa **Plan** para definir un enfoque antes de implementarlo, **Interactive** para mantenerte al tanto de los cambios específicos y **Autopilot** para tareas bien acotadas. Elige un modelo más rápido para ediciones rutinarias y uno más avanzado para tareas complejas.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad *qué* quieres crear, *por qué* y *cómo* influye significativamente en el resultado.

## Más cosas por explorar

Has completado el flujo principal. Aquí tienes algunas funciones más de CLI que puedes probar:

- `/review` para pedir al agente de revisión de código que analice los cambios;
- `/rubber-duck` para explicar un problema y obtener otra perspectiva;
- `/fleet` para coordinar subtareas independientes en paralelo;
- `/worktree` para aislar otra tarea;
- `/delegate` para enviar una tarea al agente en la nube de Copilot.

## Siguientes pasos

La mejor forma de mejorar con cualquier herramienta es seguir utilizándola. Úsala con código de producción, proyectos personales o esa pequeña aplicación que llevas años pensando en crear. Comparte lo aprendido con el equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Incorporar Foundry][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente en la nube de Copilot][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
