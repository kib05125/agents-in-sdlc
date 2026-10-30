---
title: "Lección 5: Personaliza y utiliza una habilidad de comprobaciones de calidad"
description: "Explora la habilidad quality-checks existente, personaliza el formato del informe y utilízala para validar los filtros."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar no consiste únicamente en escribir código. Ya hemos comprobado manualmente que funciona y hemos utilizado archivos de instrucciones para garantizar que siga nuestros estándares. Pero ¿qué ocurre con las pruebas, el lint y el resto de la integración continua (CI)?

Para estas tareas, las **habilidades de agente** son la opción ideal. Ayudan a Copilot a entender cómo debe ejecutar operaciones como estas.

En esta lección, aprenderás a:

- explorar la habilidad `quality-checks` existente;
- personalizar el formato de los resultados;
- volver a cargar la habilidad y ejecutarla.

## Escenario

Tailspin Toys utiliza la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones de tareas reutilizables, scripts ejecutables y recursos auxiliares que el agente carga cuando los necesita. En esencia, son carpetas con el nombre de la habilidad y un archivo Markdown llamado `SKILL.md`. El archivo Markdown incluye metadatos con un nombre y una descripción, una descripción general de la habilidad y directrices sobre cuándo debe utilizarse. La carpeta también puede incluir subcarpetas con scripts y otros recursos.

> [!NOTE]
> Una habilidad no necesita carpetas ni archivos adicionales. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md` porque utiliza los comandos existentes del proyecto.

Las habilidades pueden estar en la carpeta `.github/skills` del proyecto para compartirlas y reutilizarlas como recursos del repositorio, o en la carpeta de habilidades del usuario `~/.copilot/skills`.

## Explora la habilidad

Vamos a explorar la habilidad `quality-checks` que creó el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve al Codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee los campos `name` y `description` del principio. La descripción ayuda a Copilot a saber cuándo debe utilizar la habilidad.
3. Lee las instrucciones y observa cómo guían a Copilot durante las pruebas y el lint.
4. Fíjate en que la habilidad todavía no incluye la sección **Results output formatting**.

## Ejecuta la habilidad antes de hacer cambios

Puedes llamar a las habilidades directamente desde Copilot CLI o utilizando lenguaje natural. Pide a Copilot que ejecute las tres comprobaciones de la habilidad.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Envía esta solicitud:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observa el informe final.

## Personaliza el informe

Mejoremos el informe para que indique qué se ejecutó, si se completó correctamente y qué resultados comunicaron las herramientas.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade al final del archivo esta sección:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guardará automáticamente.

## Ejecuta la habilidad actualizada

Vamos a probar el cambio. Copilot CLI puede volver a cargar las habilidades editadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Envía exactamente la misma solicitud que antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observa el informe final y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y utilizado una habilidad de agente existente. En esta lección has:

- explorado la habilidad `quality-checks` para pruebas unitarias, lint y comprobaciones de tipos;
- personalizado el formato de los resultados;
- vuelto a cargar y ejecutado la habilidad.

Este cambio acompañará a los filtros en la PR de la función. A continuación, permitirás que Copilot interactúe directamente con el sitio [mediante el servidor MCP de Playwright][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad son referencias, no tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de trabajo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación integrado: `drawio`][drawio-example]
- [Pruebas de navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
