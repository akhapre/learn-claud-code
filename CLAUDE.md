# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Git Workflow

After completing any meaningful unit of work, commit and push to GitHub so progress is never lost:

```bash
git add <specific files>
git commit -m "short imperative description of what changed and why"
git push origin main
```

Commit messages should be concise and specific (e.g., `add wall collision to shooter player` not `update code`). Commit at logical checkpoints — after a feature works, a bug is fixed, or a refactor is complete — not in bulk at the end.

## Running the Games

No build step required. Open either HTML file directly in a modern web browser:

- `tictactoe.html` — 2-player Tic-Tac-Toe
- `shooter.html` — Top-down shooter with 4 progressive difficulty levels

## Project Structure

Two standalone single-file games. All HTML, CSS, and JavaScript lives inside each `.html` file — no external assets, dependencies, or build tools.

## Architecture

### tictactoe.html
- Board state stored as a 9-element array
- `WINS` constant defines all 8 winning combinations for linear win checking
- `init()` resets state; `checkWin()` validates conditions after each move

### shooter.html (~978 lines, organized in 10 labeled sections)

1. **Constants & Colors** — game parameters, color palette
2. **Web Audio** — lazy-initialized procedural audio (oscillator-based SFX: shoot, explode, hurt, levelup, win)
3. **Input Handler** — keyboard (WASD/arrows) + mouse tracking; `justPressedKeys` set for frame-boundary input
4. **Sprite Drawing** — all graphics drawn procedurally with Canvas 2D primitives; no image assets
5. **Entity Classes** — `Player`, `Enemy`, `Bullet` with circle-circle collision detection
6. **Particle System** — fixed pool of 200 particles (avoids GC pauses)
7. **Level Config** — 4 levels with escalating kill targets and enemy counts; Sector 4 is a 90-second survival challenge
8. **State Machine** — states: `MENU → PLAYING → LEVEL_COMPLETE → GAME_OVER / WIN`
9. **Renderers** — per-state drawing functions for HUD, menus, overlays
10. **Game Loop** — `requestAnimationFrame` with delta-time capped at 1/20s for frame-rate-independent physics

**Enemy types:** Walkers (2 HP, 65 px/s) and Runners (1 HP, 130 px/s)
**Rendering order:** background → particles → enemies → player → bullets → HUD
