# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A small collection of standalone browser games. Each game is a single self-contained `.html` file with inline `<style>` and `<script>` — no build step, no package manager, no external dependencies or network requests. Only `topdown-shooter.html` is currently tracked in git; `chalk-court.html` exists in the working directory but is not part of the repo.

## Commands

There is no build, lint, or test tooling. To run a game, open its `.html` file directly in a browser (double-click, or `start <file>.html` on Windows). There is no automated test suite — verify changes by opening the file and interacting with it manually (or via browser automation tools if available).

## Architecture

### `topdown-shooter.html`

Canvas 2D top-down shooter. Everything lives in one IIFE inside the page's `<script>` block, structured as:

- **Sprite generation** — player and enemy art is not loaded from image files. Each sprite is built at load time by `buildSpriteSheet()`, which calls per-frame draw functions (`drawPlayerFrame`, `drawEnemyFrame`) that rasterize pixel art onto an offscreen canvas using `pixelCircle`/`pixelRect`/`pixelDot` helpers (plain `fillRect` calls on a `FRAME`-sized grid, no anti-aliasing). The result is a real sprite sheet canvas, sliced per animation frame via `drawSprite()`'s `drawImage(sheet, sx, sy, ...)` call. Follow this pattern (grid helpers → sheet → sliced draw) if adding new sprites rather than sourcing image assets.
- **Game state** — a small set of module-scoped variables (`player`, `bullets`, `enemies`, `score`, `spawnTimer`, `fireTimer`, `isGameOver`), reinitialized by `resetGame()` (used both on load and on restart-after-death).
- **Loop** — `requestAnimationFrame`-driven `loop()` computes `dt` and calls `update(dt)` then `render()` each frame. `update()` handles input-driven movement/aiming, firing/cooldown, bullet and enemy movement, spawning, and all collision resolution (bullet↔enemy, enemy↔player) in one pass. `render()` is pure drawing with no state mutation.
- **HUD** — plain DOM elements (`.hud`, `.health-fill`, `#game-over` overlay) layered over the canvas via absolute positioning, updated from game code (`updateHud()`) rather than being canvas-drawn.

### `chalk-court.html`

A tic-tac-toe game following the same single-file, no-dependency pattern (inline CSS custom properties for theming, hand-drawn/chalk visual style, SVG overlay for board lines/win effects). Not currently committed to the repo.

## Conventions

- Keep new games as single self-contained `.html` files matching the existing pattern — inline styles and script, no external assets or CDN dependencies, playable by opening the file directly.
- Procedurally generate any pixel-art/sprite visuals in-code (see `topdown-shooter.html`'s sprite sheet approach) rather than adding binary image assets, unless the user asks otherwise.
