---
title: "Lección 8: Crea y combina la PR de la función"
description: "Revisa conjuntamente el filtrado, las instrucciones, la habilidad, el perfil de QA y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación de los filtros, las actualizaciones de instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una misma rama. Es el momento de revisarlos conjuntamente y abrir una solicitud de incorporación de cambios. Tú combinaste la solicitud de incorporación de cambios (PR) de las valoraciones con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente, dividiríamos la función, las actualizaciones de instrucciones, la actualización de la habilidad y el agente de QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de trabajo de filtrado y calidad en una conversación y una rama, y lo incluirás en esta PR.

En esta lección, aprenderás a:

- entender qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación;
- inspeccionar todos los cambios de la función y las pruebas de verificación;
- crear la PR de filtrado;
- habilitar Agent Merge solo después de revisar los cambios y confirmar que la PR se ha combinado.

## Escenario

Durante el flujo de trabajo de filtrado, has utilizado Copilot para planificar, implementar y verificar una función. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener bajo el control del desarrollador la autorización para combinarla.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para integrar una solicitud de incorporación de cambios. Cuando lo habilitas, Copilot resuelve los problemas que impiden combinar la PR: corrige las comprobaciones de integración continua (CI) fallidas, responde a comentarios de revisión y vuelve a basar la rama cuando es necesario. Después, habilita la combinación automática de GitHub si el repositorio lo permite.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de esa responsabilidad. Revisa el trabajo y confirma que está listo antes de habilitar Agent Merge.

## Usa Agent Merge para gestionar la PR

Ya está todo el código. Vamos a revisarlo, crear la PR y permitir que Agent Merge gestione el resto del proceso.

1. Vuelve al Codespace.
2. Abre el cuadro de diálogo de agentes escribiendo `/agent`.
3. Selecciona **Default** en la lista de opciones y pulsa <kbd>Enter</kbd>.
4. Crea una PR con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todo termine correctamente, realizará la combinación.
7. Comprueba que Copilot muestre un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no elude las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si se bloquea, lee el motivo indicado y completa manualmente la combinación revisada cuando el repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la validación del código, y ahora el proceso de las solicitudes de incorporación de cambios. En esta lección has:

- aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación;
- inspeccionado todos los cambios de la función y las pruebas de verificación;
- creado la PR de filtrado;
- habilitado Agent Merge únicamente después de revisar los cambios y confirmado que la PR se ha combinado.

A continuación, [explorarás otros comandos slash útiles de Copilot CLI][next-lesson] para gestionar contexto, modelos, sesiones compartidas y la delegación opcional a la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
