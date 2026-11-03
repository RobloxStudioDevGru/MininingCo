# Assets: contrato entre código y arte

El código busca cada asset por su carpeta y su nombre. Si un nombre cambia, el juego deja de encontrarlo. Cualquier cambio a esta lista se acuerda entre los dos antes de hacerlo.

**Dev B:** deja cada asset terminado en su carpeta, con el nombre exacto de la tabla, y avisa a Cesar.
**Dev A / Claude CLI:** mientras un asset no exista, el código crea un reemplazo gris con el mismo nombre y lo marca aquí como `reemplazo`.

## Dónde va cada cosa en Studio

| Carpeta en Studio | Qué contiene |
| --- | --- |
| `ReplicatedStorage.Assets.Blocks` | Una Part plantilla por tipo de cubo. El código la clona para dibujar la mina |
| `ReplicatedStorage.Assets.Ores` | El trozo de mineral que cae al romper un cubo |
| `ReplicatedStorage.Assets.Tools` | Picos (objetos `Tool` con una pieza llamada `Handle`) |
| `ReplicatedStorage.Assets.Equipment` | Mochilas, botas, casco, linterna, detector, explosivos |
| `ReplicatedStorage.Assets.Lighting` | Un preset de ambiente por mina (ver abajo) |
| `Workspace.Map.Hub` | La superficie central |
| `Workspace.Map.Mines` | El edificio o entrada de cada mina, con su marcador de origen |

Nunca pongas nada dentro de `ServerScriptService.Server`, `StarterPlayerScripts.Client` ni `ReplicatedStorage.Shared`: esas carpetas las controla Rojo y borra lo que no viene del repositorio.

## Lista de assets

Tamaño del cubo: **por definir en la Fase 1** (propuesta: 4 × 4 × 4 studs). Las plantillas de `Blocks` deben tener ese tamaño exacto.

### Cubos (`Assets.Blocks`)

| Nombre | Mina | Estado |
| --- | --- | --- |
| `Dirt` | Todas (capa superior) | pendiente |
| `Stone` | Todas | pendiente |
| `Copper` | Cantera, Cueva | pendiente |
| `Iron` | Cueva, Mina profunda | pendiente |
| `Gold` | Mina profunda, Caverna de cristal | pendiente |
| `Crystal` | Caverna de cristal, Núcleo | pendiente |
| `Diamond` | Núcleo | pendiente |

Cada plantilla es una `Part` anclada (`Anchored = true`) con su material, color o textura. Nada de scripts dentro.

### Minerales que caen (`Assets.Ores`)

`Ore_Stone`, `Ore_Copper`, `Ore_Iron`, `Ore_Gold`, `Ore_Crystal`, `Ore_Diamond`. Modelos pequeños (cabe en la mano). Estado: pendiente.

### Picos (`Assets.Tools`)

| Nombre | Requisito que abre | Estado |
| --- | --- | --- |
| `Pickaxe_Starter` | Cantera | pendiente |
| `Pickaxe_Stone` | Cueva | pendiente |
| `Pickaxe_Iron` | Mina profunda | pendiente |
| `Pickaxe_Gold` | Caverna de cristal | pendiente |
| `Pickaxe_Crystal` | Núcleo | pendiente |

Cada pico es un `Tool` con una `Part` o `MeshPart` llamada `Handle` donde lo agarra la mano. El resto del modelo va soldado (`WeldConstraint`) al `Handle`, con `CanCollide = false` y `Massless = true`.

### Equipo (`Assets.Equipment`)

`Backpack_1` a `Backpack_5`, `Boots_1` a `Boots_5`, `Helmet`, `Lantern_1` a `Lantern_3`, `Detector`, `Explosive`, `Elevator`. El número de niveles es provisional y se ajusta al llenar `Config`. Estado: pendiente.

### Ambiente de las minas (`Assets.Lighting`)

Una carpeta por mina, `Mine_1` a `Mine_5`, con un objeto `Atmosphere` y los valores de `Lighting` que quieras para esa mina (como atributos de la carpeta: `Ambient`, `Brightness`, `ClockTime`, `FogEnd`…). El código los aplica cuando el jugador entra.

### Superficie (`Workspace.Map.Hub`)

Piezas con estos nombres exactos, que el código usa como puntos de interacción:

| Nombre | Qué es |
| --- | --- |
| `SpawnLocation` | Donde aparece el jugador (`Neutral = true`) |
| `SellStation` | Estación de venta principal |
| `UpgradeShop` | Tienda de mejoras |
| `Leaderboard` | Donde se muestra el ranking (una `Part` plana; el código pone la interfaz encima) |
| `MineEntrance_1` a `MineEntrance_5` | Puerta de cada mina |

El resto del hub (edificios, decoración) puede llamarse como quieras.

### Minas (`Workspace.Map.Mines`)

Una carpeta por mina (`Mine_1` a `Mine_5`) con el edificio o la cueva de entrada y una `Part` invisible llamada `GridOrigin`: la esquina desde donde el código genera la rejilla de cubos. No construyas dentro del volumen de la rejilla; el código lo llena.

### Iconos

Los iconos de minerales y mejoras se suben a Roblox como imágenes. Apunta aquí el ID de cada uno y Cesar lo pasa a `Config`.

| Icono | ID de asset |
| --- | --- |
| (vacío) | |

## Límites técnicos para modelar

- Mallas: máximo 20.000 triángulos por `MeshPart` (límite de Roblox). Para objetos de mano y minerales, apunta a menos de 2.000.
- Texturas: Roblox las reduce a 1024 × 1024 como máximo; no hace falta más.
- Escala: el personaje mide unos 5 studs. Prueba cada objeto junto a un personaje antes de entregarlo.
- Desde Blender: exporta en `.fbx` y usa el importador 3D de Studio (**Inicio → Importar**). Aplica escala y rotación en Blender antes de exportar.
- Sin scripts dentro de los modelos. Si algo debe moverse o brillar, apúntalo y lo programa Cesar.
