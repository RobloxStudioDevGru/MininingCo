# Deep Mining Co.

Juego de Roblox: tycoon de minería competitivo. Cada jugador es empleado de una compañía minera: excava minas de cubos, vende minerales, mejora su equipo y compite por el ranking de la compañía. Idioma de trabajo: español (nombres de código en inglés).

Documento de diseño completo: `docs/diseno.md`. Si algo de este archivo contradice al documento, avisa antes de decidir.

## Quién hace qué

- **Cesar (Dev A)** programa todo, servidor y cliente, con Claude CLI. Revisa y fusiona las ramas.
- **Dev B** hace el arte en Studio o Blender: modelos de objetos, texturas, mapa de la superficie y ambiente de las minas. No toca código.
- **Claude CLI** escribe los `.luau` de este repositorio. Nunca mueve, renombra ni borra lo que hizo el Dev B en Studio; si el código necesita un asset que no existe, usa un reemplazo gris con el nombre acordado y lo apunta en `docs/assets.md`.

El contrato entre código y arte es `docs/assets.md`: dónde va cada modelo y cómo se llama. El código busca los assets por nombre, así que no se cambia sin avisar.

## Pilares del diseño

1. Excavar debe sentirse bien: golpear, romper cubos, ver caer el mineral.
2. Progresión clara: cada mejora abre algo nuevo (minas de mayor nivel, más capacidad, más velocidad).
3. Competencia sin perder objetos: vetas disputadas, eventos con tiempo, rankings por nivel de mina, cierre de mina con cuenta atrás.
4. El contenido vive en datos: minerales, picos y minas son tablas de configuración.
5. Tono visual intermedio entre cartoon y realista.

## Reglas técnicas (no negociables)

- **El servidor es la autoridad.** El cliente solo dibuja y pide cosas; el servidor valida todo: minado (distancia, herramienta, dureza, tiempo), ventas, compras, topes de dinero y de inventario. Nunca confiar en valores que envíe el cliente.
- **RemoteEvents con límite de frecuencia** y validación de tipo y rango de cada argumento en el servidor.
- **Un ModuleScript por sistema.** Lógica pura (rejilla, economía, topes) separada del código que usa APIs de Roblox, para poder probarla sin abrir Studio.
- **Datos en configuración, no en el código.** Minas, minerales, precios, topes y mejoras viven en `src/shared/Config`. Cambiar el balance no debe requerir tocar la lógica.
- **Persistencia con DataStore**, con manejo de errores y reintentos; nunca perder el progreso por un fallo de red. Guardar al salir y cada cierto tiempo.
- **Mundo de cubos como rejilla de datos en el servidor** (un número por posición), dividida en chunks. Para el MVP solo se dibujan como Parts los cubos expuestos. Una rejilla por mina, creada cuando entra el primer jugador y descartada cuando queda vacía.
- **Rendimiento primero:** antes de construir más sistemas, medir cuántos cubos y chunks aguanta el servidor.

## Estructura y Rojo

Rojo sincroniza los archivos del repositorio con Studio (`default.project.json`):

| Carpeta del repo | En Studio |
| --- | --- |
| `src/server` | `ServerScriptService.Server` |
| `src/client` | `StarterPlayer.StarterPlayerScripts.Client` |
| `src/shared` | `ReplicatedStorage.Shared` (incluye `Config`) |
| `docs/` | No se sincroniza: diseño, assets y prompts |

- Nombres de archivo: `Nombre.server.luau` (Script), `Nombre.client.luau` (LocalScript), `Nombre.luau` (ModuleScript). Una carpeta con `init.luau` es un ModuleScript cuyos hijos son los demás archivos.
- Rojo borra en Studio lo que no está en el disco dentro de las carpetas que sincroniza. Nada hecho a mano va dentro de `Server`, `Client` ni `Shared`.
- Los assets del Dev B viven fuera de esas carpetas (`ReplicatedStorage.Assets`, `ServerStorage.Assets`, `Workspace.Map`; detalle en `docs/assets.md`). Rojo no los toca.
- Los archivos `.rbxl` y `.rbxlx` no se suben a git: el mapa vive en el lugar de Roblox.

## Comandos

```
rokit install          # instala las herramientas fijadas en rokit.toml (Rojo 7.7.1)
rojo serve             # sincroniza con Studio; en Studio: Plugins → Rojo → Connect
rojo build -o DeepMiningCo.rbxlx   # comprueba que el proyecto compila sin abrir Studio (el archivo está en .gitignore; bórralo al terminar)
```

Con Team Create, solo Cesar conecta Rojo.

## Cómo comprobar el trabajo

- Al terminar un cambio, `rojo build` debe pasar sin errores.
- Si el MCP de Roblox Studio está conectado, úsalo para probar: iniciar el playtest, leer el Output y revisar la jerarquía. Ten en cuenta que `execute_luau` hace `require` con otra caché que el juego en marcha.
- Si no puedes probarlo en Studio, dilo en el resumen y deja a Cesar los pasos exactos para probarlo él.
- Pruebas de la lógica pura sin Studio: por decidir en la Fase 1 (candidato: Lune).

## Flujo de trabajo

- Los scripts viven solo en este repositorio. No se editan dentro de Studio.
- Una rama por tarea (`fase1/chunk-prototipo`, `fase1/inventario`…), cambios pequeños, y Cesar fusiona a `main`.
- Mensajes de commit en español, cortos y en imperativo.
- Antes de crear un sistema nuevo, revisa `docs/diseno.md` y lo que ya existe en `src/`.
- Las tareas grandes llegan como prompt en `docs/prompts/`. Al terminar una, anota el resultado y lo que quedó pendiente en `docs/progreso.md`.

## Roadmap del MVP (8 semanas)

1. **Una mina** (actual): rejilla, excavar, inventario, vender. Criterio de salida: una mina que se cava y se reinicia.
2. **Economía**: mejoras, minerales, progreso guardado.
3. **Competencia**: vetas disputadas, evento cronometrado, ranking, cierre de mina, varias minas.
4. **Pulido**: sonido, efectos, balance, prueba con jugadores reales.

Fuera del MVP: sabotaje, rebirth, clanes, precios dinámicos y tienda de cosméticos.

## Monetización (después del MVP)

Solo cosméticos y comodidad (uniformes, skins de pico, estelas, mochila extra, auto-venta). Nada que dé ventaja injusta en la competencia. Si algún día hay objetos de pago aleatorios, revisar antes la política de Roblox.
