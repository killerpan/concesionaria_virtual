# Concesionaria RPG

Demo interactivo tipo RPG pixel-art para explorar concesionarias de Chileautos. El usuario navega un personaje por 3 escenarios temáticos, se acerca a los vehículos para ver su ficha técnica y puede abrir la búsqueda en la web directamente desde el juego.

## Cómo ejecutar

No requiere build, dependencias ni servidor. Abrir directamente en el navegador:

```
# Windows
start concesionaria_demo.html

# macOS
open concesionaria_demo.html
```

## Controles

| Tecla | Acción |
|---|---|
| ↑ ↓ ← → | Mover personaje |
| ENTER / SPACE | Entrar a concesionaria seleccionada |
| ESC | Volver al mapa de selección |
| Click | Mover personaje al punto indicado |

## Estructura

Aplicación de un solo archivo (`concesionaria_demo.html`). Todo el HTML, CSS y JavaScript está inline. Sin dependencias externas ni paso de compilación.

```
concesionaria_demo.html   ← aplicación completa
img/
  chileautos.svg          ← logo
  concesionaria_edif.png  ← imagen de edificio para el mapa
  splash.jpg              ← pantalla de inicio
  autos/                  ← imágenes de los 18 vehículos
music/
  Final_Lap_Bound.mp3     ← música de fondo
```

## Escenarios

| Escenario | Marca | Vehículos |
|---|---|---|
| Izquierda | Chevrolet | Silverado, Sail, Tracker, Colorado, Captiva, Onix |
| Centro | Ford | F-150, Ranger, Explorer, Territory, Edge, EcoSport |
| Derecha | Toyota | RAV4, Hilux, 4Runner, Yaris, Corolla Cross, Corolla |

## Arquitectura

### Mapa de selección (`gameState = 'map'`)
- Tres edificios de concesionaria con animación de día/noche
- Calle animada con vehículos circulando en 2 carriles
- Personaje del jugador como indicador de selección activa
- Acceso con ENTER o SPACE

### Escenarios (`gameState = 'park'`)
- Mundo 120×35 tiles (3 escenas de 40 tiles cada una, 16×16px por tile)
- Cámara con easing que sigue al jugador
- Colisión tile-based (4 puntos del bounding box)
- NPC vendedor que se acerca al auto más cercano al jugador
- Carta de especificaciones con precio CLP, año, potencia y tipo de carrocería
- Link directo a búsqueda en Chileautos.cl al presionar la carta

### Sistema de dibujo
- `C` — paleta de colores con claves semánticas (`gD` = pasto oscuro, etc.)
- `t(x, y, w, h)` — rect alineado a tiles
- `r(x, y, w, h)` — rect alineado a píxeles
- Orden de render: fondo → estructuras → objetos → UI

### Game loop
`requestAnimationFrame` — cada frame: input → física → colisiones → render.
