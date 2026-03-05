# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running Games

Open any `.html` file directly in a browser — no build step, no server required:

```bash
cmd.exe /c start "" "C:\Users\admin\Desktop\Claude Code Test\<filename>.html"
```

## Repository Structure

This is a collection of self-contained browser games, each as a single `.html` file with inlined CSS and JavaScript. No external dependencies, no package manager, no build toolchain.

- `shooter.html` — Top-down 2D shooter (HTML5 Canvas, vanilla JS)
- `tictactoe.html` — Tic-tac-toe game

## Architecture Pattern

Every game follows the same single-file pattern:

- **CSS** inlined in `<style>` — minimal, resets only
- **Canvas + vanilla JS** — no frameworks, no imports
- **`requestAnimationFrame` loop** with delta-time (`dt`) for frame-rate-independent movement
- **State machine** via a `STATES` constant object (e.g. `MENU`, `PLAYING`, `LEVEL_COMPLETE`, `GAME_OVER`)
- **All sprites drawn programmatically** with Canvas 2D API — no image files

### shooter.html internals

Key globals: `player`, `bullets`, `enemies`, `particles`, `level`, `score`, `state`

Game loop order each frame: `update(dt)` → `draw()`

- **`update`** handles input, movement, collision, spawning, state transitions
- **`draw`** handles all rendering; branches on `state` at the top
- Enemy types defined in `ENEMY_TYPES` constant — add new types there and introduce them in `spawnEnemy()` by level
- Wave formula: `5 + (level * 3)` enemies; level bonus: `100 * level` pts on clear
- Screen shake: set `screenShake` (0–1 float); decays automatically in `update`

## Git & GitHub Workflow

**Commit and push after every meaningful unit of work.** This is non-negotiable — it ensures we never lose progress and can always revert to a known-good state. Do not batch multiple features into one commit; commit as you go.

Commit triggers (non-exhaustive):
- A new feature or mechanic is added
- A bug is fixed
- A new game file is created
- Any visible change to gameplay or UI is complete

```bash
git add <file>
git commit -m "short imperative summary

Optional longer description.

Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>"
git push
```

Commit messages must be clear and specific (e.g. `Add screen shake on player hit`, not `Update shooter`).

Remote: `https://github.com/valljo888ai/browser-games`
Default branch: `master`
