---
title: "Lección 12: Despliega el sitio con GitHub Pages"
description: "Usa la aplicación GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su solicitud de incorporación de cambios y comprobar el Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Dale un espacio público al Space Quiz y despliégalo con GitHub Pages. Copilot puede preparar el flujo de trabajo y guiar el despliegue, pero tú eres responsable de revisar los cambios y confirmar que el sitio está publicado.

En esta lección, aprenderás a:

- pedir a Copilot que configure un despliegue con GitHub Pages;
- revisar el flujo de trabajo y la solicitud de incorporación de cambios antes de combinarla;
- seguir el despliegue y comprobar que el sitio publicado está disponible.

## Escenario

El Space Quiz ya está en un repositorio de GitHub, pero las personas necesitan una forma sencilla de probarlo. Publicar el sitio de un solo archivo con GitHub Pages permite acceder a él desde una URL pública sin añadir un servidor ni una dependencia de tiempo de ejecución.

## Pide a Copilot que despliegue el sitio

En la sesión de la aplicación Copilot para `space-quiz`, envía esta solicitud:

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Diagrama de tres fases: una solicitud de incorporación de cambios de space-quiz combinada; una ejecución correcta de GitHub Actions Deploy website con los pasos Build website, Upload artifact y Deploy to Pages; y el Space Quiz publicado en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Esta solicitud pide al agente que cree y combine una solicitud de incorporación de cambios y que cambie la configuración de Pages del repositorio. Revisa el diff, los permisos de despliegue y cualquier solicitud de aprobación de GitHub antes de continuar.

## Revisa el flujo de trabajo y comprueba el despliegue

1. Confirma que el README solo se ha añadido si el repositorio aún no tenía uno.
2. Revisa los cambios de la solicitud, incluidos los pasos de compilación y despliegue del flujo de trabajo.
3. Combina la solicitud únicamente después de que pasen las comprobaciones obligatorias.
4. Sigue la ejecución del despliegue de GitHub Actions hasta que termine correctamente.
5. Abre la URL indicada y confirma que se carga el Space Quiz.

Si el agente no puede habilitar Pages ni combinar la solicitud, sigue las instrucciones específicas que te proporcione. No des por terminada la tarea hasta que el despliegue se complete correctamente y el sitio publicado se cargue.

## Resumen y siguientes pasos

El Space Quiz ya está disponible como sitio publicado y has comprobado el despliegue en vez de fiarte únicamente del informe del agente. Continúa con la [lección 13: Revisión y siguientes pasos][next-lesson].

[next-lesson]: ../13-review/
