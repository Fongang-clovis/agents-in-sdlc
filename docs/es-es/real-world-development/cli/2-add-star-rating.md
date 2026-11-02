---
title: "Lección 2 - Añadir valoraciones por estrellas: una mejora rápida"
description: "Haz un pequeño cambio en las tarjetas de juegos con Copilot CLI, revísalo en un navegador con reenvío de puertos y combínalo en tu primera PR."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ya has instalado Copilot CLI y probado una conversación. Ahora es el momento de realizar el primer cambio en el proyecto. Será pequeño: los juegos ya tienen una valoración por estrellas en sus datos, pero las tarjetas de la página de inicio todavía no la muestran. Pedirás al agente que la muestre, revisarás el cambio y lo combinarás como tu primera solicitud de incorporación de cambios.

En esta lección:

- iniciarás una conversación de Copilot centrada en una rama de funcionalidad.
- pedirás al agente que realice un pequeño cambio en el proyecto.
- revisarás el cambio con `/diff`.
- ejecutarás la aplicación para confirmar el cambio en un navegador con reenvío de puertos.
- abrirás y combinarás tu primera solicitud de incorporación de cambios.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración por estrellas, que ya aparece en la página de detalles. Sin embargo, las tarjetas de la página de inicio solo muestran el título, la categoría, el editor y la descripción. Como ejercicio inicial, pedirás al agente que muestre la valoración existente en cada tarjeta. Es un cambio pequeño y autocontenido, perfecto para tu primera sesión.

## Anatomía de una conversación

Una **conversación** es el espacio donde trabajas con Copilot CLI en una tarea. A diferencia de la aplicación Copilot, una conversación normal de CLI utiliza el repositorio y la rama de Git que están seleccionados actualmente en el terminal, en lugar de crear un árbol de trabajo específico. Las conversaciones guardadas te permiten volver a la misma conversación más tarde; los archivos y la rama siguen siendo el estado habitual de Git en el disco.

Dentro de una conversación verás tres elementos: las indicaciones y respuestas del agente, la actividad de las herramientas mientras explora y edita archivos, y los cambios que puedes examinar con `/diff`.

## Iniciar una conversación y solicitar el cambio

Iniciemos una conversación nueva para empezar a implementar la funcionalidad.

1. Vuelve a tu Codespace.
2. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Si todavía no está abierto, inicia Copilot con el comando siguiente:

   ```bash
   copilot --yolo
   ```

4. Comprueba que se inicia una sesión nueva con el comando de barra `/new` y pulsa <kbd>Enter</kbd>.
5. Utiliza esta indicación para solicitar el cambio:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot explora el proyecto, localiza los archivos que muestran los detalles de los juegos y crea el código necesario. Ya has añadido una nueva funcionalidad con Copilot CLI.

## Revisar las diferencias

Todos los cambios generados por IA deben revisarse antes de combinarlos, incluso los más pequeños. Vamos a examinar los cambios directamente en Copilot CLI.

1. Escribe `/diff` y revisa todos los archivos modificados.
2. Confirma que la tarjeta muestra la valoración numérica cuando existe y `No rating yet` cuando `starRating` es `null`.
3. Confirma que las pruebas cubren ambos casos.
4. Revisa los resultados de las comprobaciones que ha ejecutado Copilot y pídele que corrija los fallos.
5. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.

> [!NOTE]
> Copilot, como todas las herramientas de IA generativa, es probabilístico y no determinista, por lo que el código exacto puede variar. Comprueba el comportamiento en lugar de esperar una implementación idéntica.

## Comprobar los cambios

No basta con leer el código y dar por hecho que funciona. Vamos a pedir a Copilot que inicie el sitio web para poder examinar la interfaz actualizada en el navegador reenviado por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Confirma que las tarjetas de los juegos muestran sus valoraciones sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor que ha iniciado:

   ```plaintext
   Stop the development server you started.
   ```

## Abrir y combinar tu primera solicitud de incorporación de cambios

Ya has creado la funcionalidad. Es hora de crear una solicitud de incorporación de cambios (PR) para integrar el código en el proyecto.

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Escribe `/pr create`. Copilot CLI enviará el commit existente al crear la PR y mostrará su URL.
3. Abre la PR manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la URL que muestra Copilot CLI.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando esté todo listo, selecciona **Merge pull request** y confirma la combinación.
6. Vuelve a tu Codespace y sal de Copilot CLI con `/exit`.
7. Actualiza tu rama local `main`:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y pasos siguientes

Has publicado tu primer cambio con GitHub Copilot CLI. En concreto:

- has iniciado una conversación centrada en una rama de funcionalidad.
- has pedido al agente que realice un pequeño cambio en las tarjetas de los juegos.
- has revisado el cambio con `/diff`.
- has ejecutado la aplicación y confirmado la valoración por estrellas en un navegador con reenvío de puertos.
- has abierto y combinado tu primera solicitud de incorporación de cambios.

A continuación, [partirás de la incidencia de filtrado y utilizarás los modos Plan y Autopilot][next-lesson] para desarrollar una funcionalidad más amplia.

## Recursos

- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
