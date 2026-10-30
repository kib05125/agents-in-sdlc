---
slug: es-es/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Crea, verifica y publica dos cambios en Tailspin Toys mientras aprendes sobre los modos, las personalizaciones, las herramientas MCP y la automatización de solicitudes de incorporación de cambios de Copilot CLI."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** incorpora GitHub Copilot a tu terminal como asistente de programación con agentes. Explora bases de código, genera código, ejecuta comandos y se conecta a herramientas externas, todo desde la línea de comandos, para que puedas mantener el flujo sin cambiar a un editor gráfico.

El taller sigue un flujo de trabajo continuo de Tailspin Toys:

1. Prepara el proyecto en GitHub Codespaces, instala Copilot CLI y familiarízate con el entorno.
2. Añade valoraciones con estrellas, revisa el resultado en el navegador y combina manualmente tu primera solicitud de incorporación de cambios (PR).
3. Parte de la incidencia de filtrado, define el enfoque en modo Plan, impleméntalo en modo Autopilot y revísalo en modo Interactive.
4. Actualiza las instrucciones del repositorio y aplícalas al trabajo de filtrado.
5. Personaliza la habilidad `quality-checks` existente y úsala para ejecutar las comprobaciones del proyecto.
6. Añade el servidor Model Context Protocol (MCP) de Playwright y utilízalo para explorar los filtros en un navegador.
7. Crea un agente personalizado de control de calidad (QA) y úsalo para revisar requisitos, cobertura y pruebas de verificación.
8. Revisa todo el cambio de filtrado y utiliza Agent Merge para combinar la PR.
9. Explora comandos slash útiles para gestionar el contexto, los modelos, las sesiones compartidas y la delegación opcional a la nube.

Para mantener el taller centrado, crearás dos PR: una para las valoraciones con estrellas y otra para los filtros con las actualizaciones de instrucciones, la habilidad, el perfil de QA y las pruebas. El trabajo de filtrado y calidad comparte una conversación y una rama para que puedas aprovechar los cambios realizados mientras exploras cada herramienta.

## Lecciones

| Lección | Tema | Descripción |
| ------ | ----- | ----------- |
| [0. Requisitos previos][ex0] | Configuración | Crea tu repositorio y Codespace |
| [1. Instala Copilot CLI][ex1] | Instalación | Instala y autentica Copilot CLI, y familiarízate con el entorno |
| [2. Añade valoraciones con estrellas: una mejora rápida][ex2] | Primer cambio | Muestra las valoraciones existentes y la alternativa para juegos sin valorar; después, combina tu primera PR |
| [3. Modos de agente: Plan y Autopilot][ex3] | Modos de agente | Planifica la función a partir de su incidencia, impleméntala con Autopilot y revísala en modo Interactive |
| [4. Guía a Copilot con instrucciones personalizadas][ex4] | Contexto | Explora y actualiza las instrucciones y aplícalas a los filtros |
| [5. Personaliza y utiliza una habilidad de comprobaciones de calidad][ex5] | Comprobaciones repetibles | Explora la habilidad existente, cambia el formato del informe y ejecútala |
| [6. Valida la funcionalidad con Playwright MCP][ex6] | Observación en el navegador | Configura MCP en CLI y examina el comportamiento de los filtros |
| [7. Crea y utiliza un agente de QA][ex7] | Requisitos y cobertura | Crea y selecciona un perfil especializado y reúne las pruebas de verificación finales |
| [8. Crea y combina la PR de la función][ex8] | Revisión y combinación | Revisa todos los cambios, crea la PR y utiliza Agent Merge |
| [9. Comandos slash de GitHub Copilot CLI][ex9] | Funciones de CLI | Explora el contexto, los modelos, el uso compartido y la delegación opcional al agente en la nube |
| [10. Resumen y siguientes pasos][ex10] | Resumen | Repasa el flujo de trabajo, las personalizaciones reutilizables y otros recursos |
| [Opcional: Incorpora Foundry][foundry] | Agentes alojados | Prepara un modelo, implementa un agente fundamentado en el catálogo y conéctalo al sitio web |

## Requisitos previos

Antes de asistir a este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan activo **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] Permiso para crear un repositorio y un Codespace
- [ ] Conocimientos básicos de operaciones de terminal o línea de comandos

> [!TIP]
> ¿No tienes un plan de pago? Los estudiantes verificados pueden obtener GitHub Copilot gratis a través de [GitHub Education][student-plan]. El plan **Copilot Student** incluye las funciones de agente, MCP, revisión de código y Copilot CLI que utiliza este taller.

> [!NOTE]
> Si utilizas Copilot Business o Copilot Enterprise, comprueba que el administrador haya habilitado Copilot CLI.

## Primeros pasos

**[Empieza con los requisitos previos →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
