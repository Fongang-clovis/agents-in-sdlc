---
title: "Lección 1 - Instalar GitHub Copilot CLI"
description: "Instala y autentica Copilot CLI en tu Codespace, familiarízate con la herramienta y localiza la incidencia de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] es un potente asistente de programación con agentes que se ejecuta en tu terminal y te permite explorar bases de código, generar código, ejecutar comandos e interactuar con herramientas externas, todo desde la línea de comandos. Te permite delegar tareas, solicitar cambios y mantener la concentración. Como imaginarás, el primer paso es instalar la herramienta. Por suerte, puedes hacerlo con herramientas que ya conoces.

En esta lección:

- instalar GitHub Copilot CLI con npm.
- autenticarte con tu cuenta de GitHub.
- confiar en el repositorio del taller y probar una conversación breve.
- localizar la incidencia de filtrado mediante el servidor GitHub MCP integrado.

## Escenario

Tu equipo está empezando a usar agentes de IA para gestionar un backlog cada vez mayor. Copilot CLI lleva esa capacidad a la terminal, donde muchos desarrolladores ya trabajan habitualmente. Este ejercicio te ayudará a instalarlo, autenticarte y dejarlo listo para usarlo durante el resto del taller.

## Abrir un terminal en tu codespace

Antes de instalar Copilot CLI, tienes que abrir una ventana de terminal en tu codespace.

1. Vuelve a tu codespace si todavía no estás allí.
2. Abre una ventana de terminal pulsando <kbd>Ctrl</kbd>+<kbd>\`</kbd>.
3. Deberías ver un panel de terminal en la parte inferior de la ventana de VS Code.

## Instalar Copilot CLI

Puedes instalar Copilot CLI mediante [npm][install-npm], [WinGet][install-winget] y [Homebrew][install-homebrew]. Como GitHub Codespaces incluye Node.js preinstalado, usarás npm para instalar Copilot CLI.

1. En el terminal, comprueba que Node.js está instalado y que cumple el requisito de versión:

   ```bash
   node --version
   ```

   Deberías ver la versión 24 o posterior.

2. Instala Copilot CLI globalmente en el codespace con npm:

   ```bash
   npm install -g @github/copilot
   ```

4. Verifica la instalación:

   ```bash
   copilot --version
   ```

   Deberías ver el número de versión.

## Autenticarse con GitHub

La primera vez que lo inicies, Copilot CLI te pedirá que te autentiques con tu cuenta de GitHub.

1. Inicia Copilot CLI:

   ```bash
   copilot
   ```

2. Si se te solicita, sigue las instrucciones de autenticación con código de dispositivo y autoriza Copilot CLI.
3. Copilot CLI mostrará una solicitud para confirmar que confías en los archivos de esta carpeta:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Comprueba que la ruta corresponde a tu repositorio de Tailspin Toys y selecciona **Yes, and remember this folder for future sessions**.

> [!NOTE]
> En un codespace, es posible que ya estés autenticado a través de tu sesión de GitHub. Si Copilot CLI se inicia sin pedir autenticación, ya está todo listo.

## Familiarizarte con la herramienta

Los comandos que escribes en el prompt habitual del shell se ejecutan directamente en Codespaces. Cuando se inicia Copilot CLI, el lenguaje natural se envía al agente y los comandos de barra controlan la conversación.

1. Escribe `/model`, utiliza las teclas de flecha para seleccionar **Auto**, pulsa <kbd>Enter</kbd> y vuelve a pulsar <kbd>Enter</kbd> para confirmar.
2. Escribe `/help` para consultar los comandos disponibles en la versión instalada y pulsa <kbd>Esc</kbd> para cerrar la ayuda.
3. Haz una pregunta sencilla para comprobar que todo funciona:

   ```plaintext
   What are the key files in this project?
   ```

4. Lee la respuesta y observa cómo Copilot explora el repositorio antes de responder.
5. Escribe `/mcp list` y confirma que está disponible el servidor GitHub MCP integrado.
6. Pide a Copilot que encuentre la incidencia de filtrado:

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. Abre la URL y lee la incidencia. La utilizarás después de completar un primer cambio rápido.

> [!TIP]
> Una sesión normal de Copilot CLI trabaja en la rama seleccionada en el terminal; no crea automáticamente un árbol de trabajo. Crearás una rama de funcionalidad antes de cada cambio.

## Utilizar el acceso directo del taller

Normalmente, Copilot CLI pide permiso antes de utilizar herramientas fuera de los permisos establecidos. En este taller, lo reiniciarás con `--yolo`, un acceso directo aprobado para este entorno que evita esas solicitudes de confirmación en Codespaces y te permite centrarte en los ejercicios.

> [!CAUTION]
> `--yolo` habilita permisos automáticos completos (`--allow-all-tools`, `--allow-all-paths` y `--allow-all-urls`). Úsalo solo en un entorno aislado como Codespaces o una máquina virtual; nunca lo configures como alias predeterminado para el desarrollo diario. Consulta [Permitir y denegar el uso de herramientas][allow-all-warning] para obtener más información.

En este taller, `--enable-all-github-mcp-tools` habilita las herramientas GitHub MCP de lectura y escritura que utilizarás en lecciones posteriores para trabajar con incidencias y solicitudes de incorporación de cambios. Aunque Codespaces limita el acceso a tu ordenador local, los recursos de GitHub autenticados siguen siendo reales. Revisa los cambios antes de publicarlos o combinarlos.

1. Sal de Copilot CLI con `/exit`.
2. Reinícialo desde la raíz del repositorio:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Haz otra pregunta breve sobre el proyecto para confirmar que la conversación funciona y, después, sal con `/exit`.

Copilot guarda las conversaciones automáticamente. Más adelante, cuando cambies las instrucciones o añadas un agente, utilizarás `copilot --resume` para volver a la misma conversación y rama.

## Resumen y siguientes pasos

En esta lección:

- instalar Copilot CLI con npm.
- autenticarte con tu cuenta de GitHub.
- confiar en el repositorio del taller y probar una conversación breve.
- localizar la incidencia de filtrado mediante el servidor GitHub MCP integrado.

A continuación, [iniciarás tu primer cambio específico][next-lesson] y utilizarás Copilot CLI para mostrar una valoración por estrellas en las tarjetas de los juegos.

## Recursos

- [Instalar GitHub Copilot CLI][install-copilot-cli]
- [Acerca de Copilot CLI][about-copilot-cli]
- [Usar Copilot CLI][using-copilot-cli]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-copilot-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[install-npm]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli#installing-with-npm-all-platforms
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[using-copilot-cli]: https://docs.github.com/copilot/how-tos/use-copilot-agents/use-copilot-cli
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
