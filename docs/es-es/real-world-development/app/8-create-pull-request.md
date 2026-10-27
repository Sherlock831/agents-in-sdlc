---
title: "Lección 8 - Crea y combina la PR de la funcionalidad"
description: "Revisa en conjunto el filtrado, las instrucciones, la actualización de la habilidad, el perfil de control de calidad y las pruebas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de las instrucciones y de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una misma rama. Es hora de revisarlos en conjunto y abrir una solicitud de incorporación de cambios. Combinaste personalmente la PR de las valoraciones con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones y de la habilidad, y el agente de control de calidad en varias PR. Para simplificar el taller, has mantenido todo el flujo de filtrado y calidad en una sola sesión y rama, y lo incluirás en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación;
- inspeccionarás la PR completa de la funcionalidad y las pruebas de verificación;
- autorizarás Agent Merge solo después de revisarla y confirmarás que se ha combinado.

## Escenario

Durante el trabajo de filtrado, has usado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere ahora automatizar el trabajo restante de la PR, pero mantener bajo el control del desarrollador la autorización para combinarla.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios en la aplicación GitHub Copilot. Al habilitarlo, la sesión de la aplicación lee la solicitud y resuelve los bloqueos: corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y vuelve a aplicar los cambios sobre la rama base cuando sea necesario. Después, combina la solicitud en cuanto GitHub lo permite. Se ejecuta en segundo plano, sigue funcionando aunque reinicies la aplicación y se desactiva cuando la PR se combina.

Hasta ahora has seleccionado personalmente **Merge pull request**. Agent Merge puede encargarse de esa tarea, pero todavía necesita tu autorización explícita para editar código y combinar. Revisa las acciones permitidas y el trabajo antes de concederle permiso para combinar.

## Usa Agent Merge para gestionar la PR

Ya has creado y revisado todo el código. Ahora deja que Agent Merge gestione el proceso de la PR.

1. Usa el selector de agentes para seleccionar **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable no lo inicia por sí solo.

Se iniciará el proceso de Agent Merge. El agente:

- creará la solicitud de incorporación de cambios con un título y una descripción;
- si has iniciado la sesión desde una incidencia, la mencionará en el cuerpo de la descripción;
- volverá a aplicar los cambios o resolverá posibles conflictos con la rama de destino;
- supervisará CI para asegurarse de que se superen todas las comprobaciones;
- supervisará los comentarios de otros desarrolladores y de la revisión de código de Copilot, y hará los cambios necesarios para resolverlos;
- podrá combinar la PR automáticamente cuando todo se haya completado correctamente.

Vamos a permitir que Agent Merge combine la PR cuando se hayan superado todas las comprobaciones.

5. Revisa las diferencias de la PR y las pruebas de verificación antes de conceder permiso para combinar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Comprueba que **Merge pull request** esté marcado.
8. Pide explícitamente al agente que combine la PR:

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. Sigue el progreso y resuelve cualquier bloqueo que se indique. Antes de continuar, confirma que el estado de la PR sea **Merged**. Tener habilitado el permiso de combinación o un agente en ejecución no confirma que la PR se haya combinado.

> [!IMPORTANT]
> Agent Merge no omite las protecciones del repositorio ni los permisos que falten. Resuelve esos bloqueos antes de continuar.

## Resumen y próximos pasos

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la validación, y ahora también el proceso de la solicitud de incorporación de cambios. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación;
- has inspeccionado la PR completa de la funcionalidad y las pruebas de verificación;
- has autorizado Agent Merge solo después de revisar la PR y has confirmado que se ha combinado.

A continuación, [usarás un canvas existente y crearás uno de clasificación][next-lesson] para explorar una forma más completa de revisar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
