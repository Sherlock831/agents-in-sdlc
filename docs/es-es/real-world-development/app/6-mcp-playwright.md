---
title: "Lección 6 - Valida la funcionalidad con Playwright MCP"
description: "Configura Playwright MCP mediante Customize y observa el filtrado en un navegador desde el worktree de la funcionalidad existente."
authors:
  - geektrainer
lastUpdated: 2026-09-17
---

Como ya hemos comentado, programar no consiste únicamente en escribir código. También hay que trabajar con datos y servicios externos, y permitir que Copilot utilice automatizaciones adicionales. Para ello se usan los servidores MCP, que amplían las herramientas y los servicios disponibles en la aplicación.

En esta lección:

- aprenderás qué es el Protocolo de contexto de modelo (MCP) y cómo lo usa la aplicación GitHub Copilot;
- añadirás el servidor Playwright MCP;
- pedirás al agente que controle un navegador para explorar el filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero validar cambios en la interfaz requiere interactuar con ella. Quieres que Copilot pueda usar el sitio web como lo haría una persona y automatizar más tareas, para tener mayor confianza en que los cambios funcionan como se espera.

## ¿Qué es el Protocolo de contexto de modelo (MCP)?

El [Protocolo de contexto de modelo (MCP)][mcp-blog-post] permite que los agentes de IA se comuniquen con herramientas y servicios externos. Con MCP, los agentes pueden acceder a información actualizada en tiempo real mediante recursos y realizar acciones en tu nombre mediante herramientas.

Los agentes acceden a estas herramientas y recursos a través de un servidor MCP, que actúa como puente entre el agente de IA y los servicios externos, como las API existentes o herramientas locales como los paquetes de NPM. Cada servidor MCP representa un conjunto distinto de herramientas y recursos.

Algunos servidores MCP populares:

- **[Servidor MCP de GitHub](https://github.com/github/github-mcp-server)**: proporciona acceso a varias API para gestionar repositorios de GitHub, crear nuevos repositorios, actualizar los existentes y gestionar incidencias y solicitudes de incorporación de cambios.
- **[Servidor Playwright MCP][playwright-mcp-server]**: proporciona automatización del navegador mediante Playwright para que el agente pueda abrir páginas web, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro de MCP](https://github.com/mcp) para facilitar su descubrimiento y fomentar las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, comprueba quién lo publica y considera las implicaciones de seguridad. Usa solo servidores de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Añade el servidor Playwright MCP

Los servidores MCP se gestionan desde **Customize** en la barra lateral. Los servidores configurados para tus repositorios o Copilot CLI pueden estar ya disponibles en la aplicación, así que compruébalo antes de añadir un duplicado. La [documentación de personalización de la aplicación][customize-app] explica las opciones disponibles.

1. Selecciona **Customize** en la barra lateral.
2. Abre el menú desplegable **All types** y selecciona **MCP**. Después, comprueba en **Installed** si ya hay un servidor Playwright.
3. Si Playwright todavía no está instalado, busca **Playwright** entre los servidores disponibles y selecciona **Add server**.
4. Revisa quién publica el servidor, la configuración y las solicitudes de instalación antes de aprobarlas. Sigue las instrucciones para añadir el servidor; las directivas de la organización o la falta de requisitos previos pueden bloquear la configuración.
5. Vuelve a la sesión de filtrado en modo **Interactive** y confirma que las herramientas Playwright MCP están disponibles.

Si la configuración falla, resuelve el problema de configuración o permisos antes de continuar.

## Pide a Copilot que explore la funcionalidad con Playwright

La incidencia y las decisiones de planificación ya están en el contexto. Detén cualquier servidor de desarrollo que hayas iniciado antes de pedir a Copilot que inicie uno.

1. Usa este prompt para pedir a Copilot que valide la nueva funcionalidad:

    ```plaintext
    Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
    ```

> [!NOTE]
> No es obligatorio decirle a Copilot que utilice un servidor MCP concreto; normalmente encontrará el adecuado según el contexto actual. Aun así, nunca está de más indicarle algo que sabes que es importante.

2. Observa cómo trabaja.

Copilot iniciará el servidor, abrirá un navegador e interactuará con el sitio. Cuando termine, detendrá el servidor y te dará un informe.

## Resumen y próximos pasos

¡Enhorabuena! Has usado el servidor Playwright MCP para explorar la funcionalidad en un navegador real desde la aplicación GitHub Copilot. En resumen:

- has aprendido qué es MCP y cómo lo usa la aplicación GitHub Copilot;
- has añadido el servidor Playwright MCP;
- has pedido al agente que controle un navegador para explorar el filtrado.

A continuación, [crearás un agente personalizado de control de calidad][next-lesson] que combinará la habilidad y las herramientas del navegador en un rol especializado.

## Recursos

- [¿Qué es MCP y por qué habla todo el mundo de ello?][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [Configurar servidores MCP en la aplicación GitHub Copilot][customize-app]

[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[customize-app]: https://docs.github.com/copilot/how-tos/github-copilot-app/customize-github-copilot-app
