---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios, has pedido al agente de GitHub Copilot en la nube que realice tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente trabaja de forma autónoma y asíncrona, es importante poder supervisar estas tareas.

Dispones de varias herramientas para gestionar las tareas asignadas al agente, entre ellas la [página de agentes][agents-page] de GitHub.com. Desde este panel de control puedes ver todas las tareas de los agentes que tienen solicitudes de incorporación de cambios abiertas. También puedes explorar las operaciones realizadas e incluso orientar una sesión que sigue en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube;
- dirigirás una sesión en curso para pedir una funcionalidad adicional.

## Escenario

Después de pedir al agente que cree un modo de alto contraste, el equipo decidió que sería buen momento para añadir también un modo claro. Como ya se estaba actualizando el estilo del sitio y añadiendo controles de selección de modo, parecía lógico incluir esta funcionalidad. Vas a orientar el trabajo del agente para que añada el modo claro además del modo de alto contraste.

## Revisa las tareas del agente de GitHub Copilot en la nube

Vamos a consultar el estado de las tareas que has asignado al agente de Copilot en la nube.

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa las tareas tanto en el panel principal como en el panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código base;
   - generar API para modificar juegos;
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso y revisa las acciones que ha realizado Copilot. Pueden incluir:
   - obtener el código del repositorio;
   - crear el entorno de trabajo de Copilot;
   - configurar servidores MCP;
   - realizar distintas acciones para completar la tarea.

> [!NOTE]
> Los pasos exactos que aparecen dependen del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Observa también el panel de solicitudes de incorporación de cambios que aparece a la derecha. Desde allí puedes consultar la solicitud y los archivos modificados para seguir el trabajo.

## Dirige al agente en la nube

Ahora que has visto las tareas activas, pídele a Copilot que añada un control para el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que use Copilot y del estado actual del trabajo.

    ![Sesión de accesibilidad en el panel de control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado de configurar el entorno y ha empezado a trabajar. Verás mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este mensaje:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de la tarea del agente en la nube en la página de agentes, con el cuadro de diálogo para dirigir una sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el mensaje.
5. Observa cómo Copilot lo confirma y lo incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot seguirá con la tarea actualizada. Incorporará la nueva solicitud cuando termine el paso en el que estuviera trabajando al recibir el mensaje.

El proceso tardará varios minutos, así que puedes aprovechar para repasar todo lo que has aprendido y explorado.

## Resumen y próximos pasos

En esta lección has explorado la página de agentes de Copilot, el centro desde el que puedes supervisar y orientar las tareas del agente de GitHub Copilot en la nube. Permite hacer seguimiento de todas las tareas activas y completadas, revisar el trabajo realizado e incluso redirigir una tarea en curso para cambiar su alcance o añadir indicaciones.

Has explorado estos conceptos:

- usar el panel de control y la página de agentes para supervisar las tareas del agente en la nube;
- redirigir una sesión en curso para solicitar una funcionalidad adicional.

Cuando Copilot termine el trabajo de accesibilidad, podrás [iterar sobre sus solicitudes de incorporación de cambios][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Próxima lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
