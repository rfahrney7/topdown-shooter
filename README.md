# Top-Down Shooter

A browser-playable top-down shooter built as a single self-contained HTML file — no build tools, no dependencies, no external assets. Player and enemy sprites are pixel art generated at runtime onto canvas sprite sheets.

## Play

Open `topdown-shooter.html` directly in a browser.

## Controls

- **WASD** — move
- **Mouse** — aim
- **Click (hold)** — shoot

## How it works

- Rendered with the Canvas 2D API.
- Player and enemy sprites are procedurally drawn pixel grids, baked into offscreen sprite-sheet canvases and sliced per animation frame with `drawImage`.
- Core loop: player movement/aiming, enemy spawning and chase AI, bullet collisions, health/score HUD, and a game-over/restart flow.
