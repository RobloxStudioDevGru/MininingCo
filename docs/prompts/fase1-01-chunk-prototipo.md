# Fase 1 · Tarea 1: prototipo de chunk rompible con medición

Lee `CLAUDE.md` y las secciones "Mundo de cubos y terreno excavable" y "Arquitectura técnica en Roblox" de `docs/diseno.md` antes de empezar.

## Objetivo

Una mina de prueba hecha de cubos que el jugador puede romper con clic, dibujada solo con los cubos expuestos, y números reales de cuánto aguanta. Esta tarea decide el tamaño de cubo, de chunk y de mina para el resto del juego.

Rama: `fase1/chunk-prototipo`.

## Antes de escribir código

Preséntame un plan corto y espera mi aprobación. Incluye:
1. El formato de `src/shared/Config` para cubos y minas (qué campos tiene cada uno). Es el formato que usaremos todo el juego, así que piénsalo bien.
2. Cómo guardarás la rejilla en memoria (por ejemplo, un `buffer` por chunk) y por qué.
3. Qué módulos vas a crear y qué hace cada uno.
4. Si propones Lune u otra herramienta para probar la lógica pura sin Studio, dime cuál y qué versión; no la instales sin preguntarme.

## Qué construir

- **Config:** tipos de cubo (aire, `Dirt`, `Stone`, `Copper`, como mínimo) con su dureza, y una mina de prueba con su tamaño en cubos, tamaño de chunk y tamaño de cubo en studs.
- **Rejilla (lógica pura, sin APIs de Roblox):** leer y escribir el tipo de un cubo, saber si un cubo está expuesto (toca aire), dividir en chunks y generar capas simples por profundidad.
- **Servidor:** crea la rejilla de la mina, envía al cliente los chunks cercanos y recibe peticiones de romper un cubo. Valida coordenadas dentro de la mina, distancia al personaje, que el cubo no sea aire y un límite de frecuencia por jugador. Al romper, actualiza la rejilla y avisa a los clientes del cambio.
- **Cliente:** dibuja como `Part` solo los cubos expuestos de los chunks recibidos, clonando la plantilla de `ReplicatedStorage.Assets.Blocks.<Nombre>` si existe, o una pieza gris de reemplazo si no (ver `docs/assets.md`). Al hacer clic en un cubo pide romperlo; al confirmarse, quita esa pieza y dibuja los vecinos que quedaron expuestos.
- **Ubicación:** la rejilla empieza en `Workspace.Map.Mines.Mine_1.GridOrigin` si existe; si no, en un punto fijo de prueba.

## Medición

Prueba al menos tres tamaños de mina (de pequeña a grande) y anota en `docs/progreso.md` una tabla con: tamaño de mina y de chunk, cubos totales, piezas dibujadas, tiempo de generación, tamaño de los datos enviados por chunk, memoria del servidor y del cliente, y FPS del cliente. Termina con tu recomendación de tamaños para el MVP.

## Fuera de esta tarea

Inventario, venta, minerales que caen, DataStore, picos con niveles y reinicio de mina. Vienen en las siguientes tareas.

## Criterio de terminado

- `rojo build` pasa sin errores.
- En un playtest de Studio se rompen cubos con clic y aparecen los de detrás.
- El servidor rechaza peticiones fuera de rango o demasiado seguidas (dime cómo lo comprobaste).
- La tabla de medición está en `docs/progreso.md`.
- Si algo no pudiste probar en Studio, dímelo y deja los pasos para que lo pruebe yo.
