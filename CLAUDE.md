# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the Project

No build process. Open `concesionaria_demo.html` directly in a browser:

```
start concesionaria_demo.html   # Windows
open concesionaria_demo.html    # macOS
```

There are no dependencies, no package manager, no compile step, and no tests.

## Architecture

Single-file vanilla JS canvas application (`concesionaria_demo.html`, ~787 lines). All HTML, CSS, and JavaScript are inline.

### World Layout

Three horizontally adjacent scenes, each 40×30 tiles (16×16px per tile):
- **Left wing** — used cars
- **Center** — sports cars
- **Right wing** — luxury cars

The camera follows the player with easing; scenes scroll horizontally as the player moves between them.

### Drawing System

- `C` — color palette object with semantic shorthand keys (e.g., `gD` = grass dark, `tC` = tree center)
- `t(x, y, w, h)` — draw a tile-aligned rect
- `r(x, y, w, h)` — draw a pixel-aligned rect
- Named drawing functions: `drawScene()`, `drawCar()`, `drawPlayer()`, `drawTreeAt()`, `drawLampAt()`, `drawFlowerAt()`
- Layered render order: background → structures → objects → UI

### Game Loop

`requestAnimationFrame`-based loop. Each frame: process input → update player position + camera → collision check → draw everything.

### Collision Detection

Tile-based walkability map. Checks four corner points of the player's bounding box against non-walkable tiles.

### Interaction System

Proximity detection triggers a card UI showing specs (price, year, performance, body type) for the nearest car. Card includes a link that opens a ChileAutos search for the vehicle name in a new tab.

### Car Data

18 cars defined as objects with: name, price, year, specs, body type, and tile position. Each scene has its own themed subset.
