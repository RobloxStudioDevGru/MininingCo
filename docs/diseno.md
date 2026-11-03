# Deep Mining Co. — Documento de diseño

Nov 3, 2026 · @Cesar Alberto Cazares Chavan

## Concepto

Deep Mining Co. es un juego donde todos los jugadores cavan en minas hechas de cubos, venden lo que sacan, mejoran su equipo y compiten por las mejores vetas, los rankings y los eventos del servidor.

**La fantasía:** empiezas con un pico básico en la mina de nivel 1 y cavas en busca de minerales. Cada cubo se puede romper, así que tú decides si haces un túnel recto, una escalera o una caverna. Al mejorar tu equipo entras a minas de mayor nivel, con minerales más valiosos y rivales de tu nivel que quieren la misma veta.

**Ambientación:** cada jugador es empleado de una compañía minera. Los uniformes, el casco y el logo son cosméticos, los reinicios de mina son turnos de trabajo y el ranking es la tabla de la compañía. El nombre lleva "Co." para distinguirse de otros juegos de Roblox con nombres parecidos, como Depth Mining.

**Qué lo diferencia:** combina un tycoon de mejoras con competencia directa en minas compartidas por jugadores de un mismo nivel. Ver a otros cavar cerca, o llegar tarde a una veta, crea la tensión que un tycoon solitario no tiene.

**Supuesto de diseño:** las minas son espacios designados y delimitados, cada una con su nivel. Dentro de cada una, el terreno es una rejilla de cubos del mismo tamaño, y cualquier cubo se puede excavar con la herramienta adecuada, no solo los minerales.

**Tono visual:** un punto medio entre cartoon y realista. Cubos con texturas detalladas pero formas simples, colores vivos sin llegar a lo caricaturesco, e iluminación con ambiente propio en cada mina.

**Pilares de diseño**

1. **Minas por niveles, todo cavable.** Dentro de cada mina el jugador elige su ruta, y subir de nivel abre minas más ricas y exigentes.
2. **Competir sin perder lo que ya tienes.** Nadie roba el inventario ni el dinero de otro: se compite por vetas, puntos y premios.
3. **El primer minuto es rápido y el progreso es profundo.** Una mejora en menos de dos minutos, y metas a largo plazo siempre visibles.
4. **El contenido vive en datos.** Minerales, picos y zonas son tablas de configuración, para que dos personas puedan ampliar el juego sin escribir sistemas nuevos.

## Bucle de juego

Cada cubo que minas hace dos cosas a la vez: te da minerales para vender y mejorar, y suma puntos en las vetas disputadas, los eventos y el ranking.

&#91;embedded content: bucle de juego · dos carriles, 8 pasos\]

El punto de unión es mejorar tu equipo: con él pagas con dinero de ventas y con recompensas de competencia, y te permite entrar a una mina de mayor nivel para empezar otra vuelta.

## Economía: minerales y mejoras

El dinero sale de vender minerales, y cada mejora hace que cada viaje de minería rinda más o que llegues antes a las vetas valiosas. Los costos y precios son valores iniciales para ajustar con pruebas.

**Minerales**

| Mineral | Mina | Rareza | Valor relativo |
| --- | --- | --- | --- |
| Piedra | Cantera | Muy común | Muy bajo |
| Cobre | Cantera y cueva | Común | Bajo |
| Hierro | Cueva | Común | Medio |
| Oro | Mina profunda | Poco común | Alto |
| Cristal | Caverna de cristal | Raro | Muy alto |
| Diamante | Núcleo | Muy raro | El más alto |

**Mejoras**

| Mejora | Qué cambia | Efecto en la competencia |
| --- | --- | --- |
| Pico | Daño y velocidad al romper cubos; los picos nuevos rompen minerales más duros | Llegas antes a una veta disputada |
| Mochila | Capacidad de minerales | Menos viajes para vender |
| Botas | Velocidad y salto | Rutas más rápidas hacia la veta |
| Linterna y detector | Radio de luz y aviso de vetas cercanas | Ves las vetas antes que los demás |
| Explosivos | Rompen varios cubos de una vez, con enfriamiento largo | Abren camino rápido, pero se gastan |
| Ascensor y auto-venta | Venta sin subir a la superficie | Menos tiempo perdido |

**Reglas de la economía**

- El costo de cada mejora crece por nivel, y la primera se compra en menos de dos minutos de juego.
- Los minerales más duros exigen un pico de cierto nivel: sin él no se pueden romper, lo que ordena el progreso por niveles de mina.
- **Reinicio (rebirth):** vuelves a la superficie con un multiplicador permanente de venta y cosméticos; el equipo y el dinero se reinician.
- **Opcional después del MVP:** el precio de cada mineral baja un poco cuando el servidor mina mucho de él y sube cuando casi nadie lo mina.

## Mundo de cubos y terreno excavable

Cada mina es una rejilla 3D de tamaño fijo, guardada como datos en el servidor y dibujada solo cerca de los jugadores. Así se puede cavar cualquier cubo sin que el servidor se sature de objetos.

**Cómo funciona**

- **Rejilla de cubos:** cada posición guarda un número que indica su tipo (tierra, piedra, mineral, aire). No es un objeto de Roblox, es un dato.
- **Chunks:** la rejilla se divide en bloques pequeños (el tamaño se ajusta con pruebas). Cada chunk se genera, se actualiza y se dibuja por separado.
- **Solo se dibujan los cubos expuestos:** los que tocan aire. Un cubo del interior no existe como pieza hasta que se destapa.
- **Romper un cubo:** el cliente pide romperlo; el servidor comprueba distancia, herramienta y dureza, le resta vida, y al romperse actualiza la rejilla y entrega el mineral.
- **Generación:** las capas dependen de la profundidad dentro de la mina, con cuevas hechas con ruido y vetas de minerales según la tabla de cada nivel de mina.
- **Regeneración:** cada mina se reinicia cuando se agota o cuando termina una cuenta atrás visible, y vuelve a generar vetas nuevas. Lo que se guarda entre sesiones es el progreso del jugador, no los huecos que cavó.

**Cómo dibujar los cubos**

| Opción | Ventaja | Problema | Uso propuesto |
| --- | --- | --- | --- |
| Una pieza por cubo expuesto | Fácil de programar; cada cubo puede tener textura, vida y efectos | Muchas piezas si se dibujan demasiadas a la vez | MVP, con chunks y solo cubos expuestos |
| Terreno nativo de Roblox | Excavable de fábrica y de aspecto natural | Menos control del estilo de cubos y de cada mineral | Descartado para el MVP |
| Malla generada por código por chunk | Mejor rendimiento en mundos grandes | Más compleja; hay que revisar qué permite Roblox en ese momento | Solo si el rendimiento lo exige después |

**Límites del MVP:** minas de tamaño fijo y moderado, y un solo tamaño de cubo. Cambiar el tamaño de una mina o añadir otra es un cambio de datos, no de sistemas.

## Competencia

Se compite por velocidad, puntos y premios, no por quitarle cosas a otros. Así ganar se siente bien y perder no hace que nadie abandone el juego.

| Modo | Cómo funciona | Qué se gana |
| --- | --- | --- |
| Vetas disputadas | Aparece una veta grande y visible para todos en la mina. El daño que cada jugador aporta se registra, y el premio se reparte según ese aporte, con un bono extra para quien la termina | Minerales extra y puntos |
| Fiebre de oro | Evento cronometrado de pocos minutos: gana quien más valor mine en ese tiempo | Puntos y bonos temporales |
| Ranking semanal | Tabla por valor minerado, profundidad máxima y vetas completadas | Cosméticos y títulos |
| Cierre de mina | La mina se reinicia con una cuenta atrás visible y, en los minutos finales, aparecen vetas ricas que todos quieren vaciar antes del cierre | Dinero y puntos |

**Sabotaje ligero (opcional, desde la fase 4)**

- Un explosivo que rompe cubos en una zona, para estorbar el camino a otro sin dañar a la persona.
- Un cebo que atrae a los jugadores a una veta falsa, de efecto corto.

**Reglas de equidad**

| Problema | Regla propuesta |
| --- | --- |
| Los jugadores con mejor equipo siempre ganan | Cada mina tiene un nivel y un requisito de equipo, así que solo coinciden jugadores con equipo parecido; los rankings se separan por nivel de mina |
| Un jugador nuevo no puede participar | Las vetas disputadas dan algo a todos los que aportaron daño, no solo al ganador |
| Alguien bloquea túneles a propósito | Los jugadores atraviesan a otros dentro de los túneles, y los cubos que pone un jugador no pueden bloquear una veta disputada |
| Alguien pide ayuda de otra cuenta para ganar puntos | Los puntos de ranking solo cuentan minerales minados por uno mismo, y los topes por evento limitan el abuso |
| Trampas (velocidad, romper desde lejos) | El servidor comprueba distancia, herramienta y tiempo de cada rotura |

**Principio de diseño:** cualquier forma de competir debe poder jugarse sin que nadie pierda su progreso. Lo peor que puede pasar es perder un premio, nunca el equipo ni el dinero.

## Minas designadas y niveles

Subir de nivel de mina es el eje del progreso: cada mina exige un pico mejor y un equipo más completo, y paga más. Los peligros se resuelven con equipo, no con reflejos, para que el juego siga siendo de tycoon.

| Nivel | Mina | Minerales principales | Peligro | Requisito para entrar |
| --- | --- | --- | --- | --- |
| 1 | Cantera | Piedra y cobre | Ninguno | Pico inicial |
| 2 | Cueva | Cobre y hierro | Poca luz | Pico de piedra |
| 3 | Mina profunda | Hierro y oro | Oscuridad y aire pesado | Pico de hierro y linterna mejorada |
| 4 | Caverna de cristal | Oro y cristal | Cubos frágiles que se rompen al pisarlos | Pico de oro y botas mejoradas |
| 5 | Núcleo | Cristal y diamante | Calor que daña poco a poco | Pico de cristal y protección contra calor |

El tamaño, la capacidad y los minerales de cada mina son datos de configuración y se ajustan con pruebas.

**Cómo funcionan las minas designadas**

- **Superficie central:** es el punto de partida, con la tienda de mejoras, las estaciones de venta, el ranking y la entrada a cada mina.
- **Minas delimitadas:** cada mina es un espacio de tamaño fijo con su propio nivel, minerales, peligro y capacidad de jugadores.
- **Entrada por equipo:** se entra a una mina al cumplir su requisito, y al subir de nivel se desbloquea la siguiente.
- **Competencia pareja:** en una mina solo coinciden jugadores con equipo parecido, así que las vetas disputadas son justas.
- **Capacidad limitada:** cada mina admite pocos jugadores a la vez, y se abre otra copia si se llena.
- **Reinicio de mina:** cada mina se reinicia con una cuenta atrás visible, lo que crea una carrera por sacar lo mejor antes del cierre.
- **Estación de venta en la entrada** de cada mina, para no subir siempre a la superficie.

## Arquitectura técnica en Roblox

El cliente solo dibuja los cubos y pide acciones; el servidor guarda cada mina, valida cada rotura y entrega los minerales. Es la defensa principal contra trampas y evita que el servidor cargue minas vacías.

&#91;embedded content: arquitectura · cliente, servidor, datos y configuración\]

Cada módulo del servidor tiene una responsabilidad y se comunica con el cliente únicamente por RemoteEvents.

**Reglas técnicas**

- **Una rejilla por mina:** cada mina es un objeto de datos aparte, que se crea cuando entra el primer jugador y se descarta cuando queda vacía.
- **Chunks bajo demanda:** el servidor solo envía al cliente los chunks cercanos al jugador, y el cliente dibuja solo los cubos expuestos.
- **Nunca confiar en el cliente:** distancia, herramienta, tiempo de rotura, minerales y dinero se calculan en el servidor.
- **Un ModuleScript por sistema,** con funciones claras que el otro dev pueda llamar (pedir un chunk, romper un cubo, mostrar un aviso).
- **Valores en una configuración compartida:** tamaño y capacidad de cada mina, minerales, precios y costos viven en una tabla, no en el código.
- **Guardado:** al salir el jugador y cada cierto tiempo, con reintentos si el DataStore falla.
- **Límite de frecuencia en los RemoteEvents** para que nadie pueda saturar al servidor con peticiones.

## Reparto de trabajo para 2 devs

Reparto decidido por Cesar el 3 de noviembre de 2026: el **Dev A (Cesar, con Claude CLI)** programa todo, servidor y cliente. El **Dev B** hace el arte: modelos de objetos, texturas, el mapa de la superficie y el ambiente de cada mina. El código no lo toca el Dev B, y el Dev A no modela.

| Área | Responsable | Entregable |
| --- | --- | --- |
| Datos y guardado | Dev A | Dinero, equipo, minerales descubiertos y nivel |
| Rejilla de cubos y generación | Dev A | Minas por capas con cuevas, vetas y reinicio |
| Minería en servidor | Dev A | Romper cubos con validación y entrega de minerales |
| Inventario, venta y mejoras | Dev A | Mochila, estaciones de venta y tienda de mejoras |
| Competencia | Dev A | Vetas disputadas, eventos, ranking y reglas de equidad |
| Dibujo de chunks | Dev A | Cubos expuestos que aparecen y desaparecen según la cercanía |
| Interfaz (código) | Dev A | Mochila, tienda, ranking y avisos de eventos |
| Efectos y sonido (código) | Dev A | Disparar partículas y sonidos al romper, vender y mejorar |
| Modelos de objetos | Dev B | Picos por nivel, mochilas, botas, casco, linterna, detector, explosivos, ascensor |
| Texturas de cubos y minerales | Dev B | Una textura por tipo de cubo (tierra, piedra y los 6 minerales) |
| Superficie y entradas | Dev B | Hub con tienda, estaciones de venta, ranking y entrada a cada mina |
| Ambiente de las minas | Dev B | Iluminación, niebla y decoración de cada uno de los 5 niveles |
| Arte de interfaz y efectos | Dev B | Iconos de minerales y mejoras, aspecto de las partículas, selección de sonidos |
| Pruebas de balance | Ambos | Sesiones semanales con amigos |

**El punto de unión más importante:** la convención de nombres y carpetas de los assets (`docs/assets.md`). El código busca cada modelo por su nombre, así que un nombre cambiado rompe el juego. Se acuerda en la primera semana y no se cambia sin avisar.

**Reglas de trabajo en pareja**

- Team Create para editar el mismo lugar. El Dev B trabaja en Studio (mapa y modelos); los scripts solo viven en el repositorio y llegan a Studio con Rojo.
- El Dev B deja cada asset terminado en la carpeta acordada con el nombre acordado, y avisa al Dev A.
- Mientras un asset no existe, el código usa una pieza gris de reemplazo con el mismo nombre, para no bloquear a nadie.
- Una reunión corta semanal para decidir qué entra a la siguiente prueba.

## Roadmap del MVP

El MVP se construye en cuatro fases de dos semanas, y no se pasa a la siguiente hasta cumplir el criterio de salida de la anterior. Primero se perfecciona una sola mina, porque es la base de todo lo demás. Las semanas son una estimación para dos personas con tiempo parcial y se reajustan tras cada fase.

&#91;embedded content: roadmap MVP · 4 fases en 8 semanas, con criterio de salida\]

**El MVP incluye:** 5 minas de distinto nivel con su superficie central, 6 minerales, 6 mejoras, vetas disputadas, un evento cronometrado y ranking por nivel de mina.

**Queda fuera hasta después:** sabotaje, reinicio (rebirth), clanes, precios dinámicos y tienda de cosméticos.

## Referencias en Roblox

Los juegos de minería de Roblox se reparten sobre todo entre simuladores de progreso individual y tycoons de automatización, y en las fuentes revisadas la competencia directa por vetas casi no aparece. Consultado el 3 de noviembre de 2026.

| Juego | Qué hace | Qué se toma de referencia |
| --- | --- | --- |
| [Mining Simulator 2](https://earnaldo.com/blog/pickaxe-tycoon-vs-mining-simulator-2) | Simulador de Rumble Studios, activo desde mayo de 2022 y con más de 2.5 mil millones de visitas | Pulido visual y variedad de sonido |
| [Pickaxe Tycoon](https://pickaxe-tycoon-b1p.pages.dev/) | Tycoon de abril de 2026 con minería automática, fusión de picos y base; 6 jugadores por servidor | El formato tycoon más parecido |
| [Miners Haven](https://imap.compagnie-des-sens.fr/dHw/478/get76ppcyRobloxMiner) | Automatización con taladros, cintas y procesadores, y rankings | Progresión larga |
| [Pickaxe Mining Simulator](https://roblox.com/games/108975504818945) | Cavar hacia abajo atravesando capas | Sensación de profundidad |
| [Depth Mining](https://www.roblox.com/games/6493587023) | Sandbox donde se puede romper cualquier bloque | Concepto y nombre cercanos |
| [Depthbound](https://www.roblox.com/games/10910523703) | Minería infinita de hasta 16,300 bloques de profundidad | Progreso por profundidad |
| [Mining Inc Remastered](https://roblox.com/games/3168928542) | Juego de equipo dentro de la mina, con más de 46 millones de visitas | Cooperación dentro de la mina |
| [Ultimate Mining Tycoon](https://deltiasgaming.com/?p=221209) | Tycoon de minería con fábricas, de Innovation Inc. | Referencia de tycoon |

**Dónde diferenciarse:** minas por nivel con jugadores de equipo parecido, vetas disputadas, cierre de mina con cuenta atrás y rankings por nivel. Es una lectura de las fuentes revisadas, no un estudio completo del mercado, así que conviene repasar Roblox Discover.

**Sobre el nombre:** no se encontró otro juego de Roblox con el nombre exacto Deep Mining Co., pero existe Depth Mining, de concepto parecido. Por eso el nombre se usa siempre completo, con "Co." y un logo propio.

## Monetización, riesgos y siguientes pasos

La monetización debe ser de comodidad y de aspecto, porque vender ventaja en una competencia hace que los demás se vayan.

**Monetización**

- **Game passes de comodidad:** mochila extra, auto-venta, detector de vetas básico.
- **Cosméticos:** uniformes de la compañía, aspectos de pico, estelas, efectos al romper y títulos de ranking.
- **No vender:** picos de nivel alto ni ventajas directas en vetas disputadas.
- Si en el futuro hay cajas con premios aleatorios de pago, revisar antes las políticas de Roblox sobre ese tipo de ítems.

**Riesgos principales**

| Riesgo | Mitigación |
| --- | --- |
| Rendimiento: demasiados cubos dibujados | Chunks, solo cubos expuestos y un límite de distancia de dibujo; medir desde la primera semana |
| Poca gente conectada: la competencia pierde sentido | Eventos que funcionan también con pocos jugadores y rankings contra el propio récord, y minas de cada nivel que se abren solo cuando hay jugadores |
| Economía desbalanceada | Precios y costos en una tabla de configuración que se ajusta sin tocar código |
| Trampas al romper cubos | El servidor valida distancia, herramienta y tiempo de cada rotura |
| Alcance demasiado grande | 5 minas, 6 minerales y 6 mejoras; todo lo demás va después del MVP |
| Se vuelve repetitivo | Vetas disputadas y eventos rompen la rutina cada pocos minutos |

**Siguientes pasos**

- [ ] Buscar Deep Mining Co. en Roblox Discover antes de fijar el nombre, y reunir referencias visuales del punto medio entre cartoon y realista.
- [ ] Acordar el reparto entre los dos y el formato de chunk.
- [ ] Probar un prototipo con una mina pequeña de cubos que se pueden romper, y medir el rendimiento.
- [ ] Programar guardado y venta básica antes de añadir competencia.
- [ ] Hacer la primera prueba con amigos y anotar dónde se aburren o se frustran.
