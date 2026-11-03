# Deep Mining Co.

Tycoon de minería para Roblox. Reglas del proyecto en `CLAUDE.md`; diseño en `docs/diseno.md`.

- **Programación (Cesar, con Claude CLI):** todo el código vive en este repo. Tareas en `docs/prompts/`, avances en `docs/progreso.md`.
- **Arte (Dev B):** modelos, texturas y mapa en Studio. Nombres y carpetas de cada asset en `docs/assets.md`. Los pasos de abajo no te hacen falta; solo entra al lugar con Team Create.

## Preparar el entorno (una vez por equipo)

1. Instala [Rokit](https://github.com/rojo-rbx/rokit): descarga `rokit-*-windows-x86_64.zip` de sus Releases, descomprímelo y ejecuta `rokit self-install`. Cierra y vuelve a abrir la terminal.
2. En la carpeta del repo: `rokit install` (instala la versión de Rojo fijada en `rokit.toml`).
3. Instala el plugin de Rojo en Studio: `rojo plugin install`.
4. En VS Code, instala la extensión **Rojo** (evaera.vscode-rojo) y **Luau Language Server** (JohnnyMorganz.luau-lsp).

## Trabajar cada día

1. En la carpeta del repo: `rojo serve`.
2. En Studio abre el lugar del juego, pestaña **Plugins → Rojo → Connect**.
3. Edita los `.luau` en VS Code; los cambios aparecen en Studio al guardar.

Los scripts solo se editan aquí, nunca en Studio. El mapa y los modelos sí se hacen en Studio.

## Dónde va cada cosa

| Carpeta        | En Studio                                    |
| -------------- | -------------------------------------------- |
| `src/server`   | `ServerScriptService.Server`                 |
| `src/client`   | `StarterPlayer.StarterPlayerScripts.Client`  |
| `src/shared`   | `ReplicatedStorage.Shared`                   |
