# GameMaker Rust

A 2D game engine with a built-in visual editor, written in Rust on top of
[Bevy](https://bevyengine.org/) (ECS + rendering) and [egui](https://github.com/emilk/egui)
(editor UI). The goal is a GameMaker-style workflow: build a scene in the editor, press
Play, test it, press Stop and you're back where you started.

> **Status:** experimental, paused since July 2025 (v0.4.0). It runs and the editor
> works, but it's a learning project, not a production engine.

## What works

- **Dockable editor** (`egui_dock`): Viewport, Hierarchy, Inspector, Asset Browser,
  Console, Entity Spawner, Game Controls, and grid/background/scene settings panels you
  can drag, resize and rearrange.
- **Inspector with live component editing**: add or remove components (Player, Enemy,
  Shooter, Collider, Health, CustomSprite) on any entity while the game is running.
- **Play / Pause / Stop**: the scene state is snapshotted when you press Play and
  restored on Stop.
- **Scene save/load** to [RON](https://github.com/ron-rs/ron) files (`scenes/`).
- **Gameplay systems**: keyboard movement, shooting with cooldowns, AABB collision,
  health and damage, screen-boundary clamping.
- **Editor camera** with right-click drag panning, toggleable grid and background.

## Architecture

```
┌──────────────────────────────┐
│ Editor UI (egui + egui_dock) │  hierarchy, inspector, asset browser, console
└──────────────┬───────────────┘
               │ reads/writes ECS state
┌──────────────▼───────────────┐
│ Engine core (Bevy ECS)       │  components, resources, systems, scene (RON)
└──────────────┬───────────────┘
┌──────────────▼───────────────┐
│ Renderer (Bevy / wgpu)       │  2D sprites, grid, background
└──────────────────────────────┘
```

| Path | Contents |
| --- | --- |
| `src/components.rs` | ECS components (Player, Enemy, Shooter, Collider, Health, …) |
| `src/resources.rs` | Editor and game state, dock layout, settings |
| `src/systems/` | Input, gameplay, collision, camera, rendering, editor shortcuts |
| `src/ui/` | Editor panels: hierarchy, inspector, asset browser, menus |
| `src/scene/` | Scene serialization to and from RON |

## Run it

Requires [Rust](https://www.rust-lang.org/tools/install).

```bash
git clone https://github.com/jimmyfnc/gamemaker_rust.git
cd gamemaker_rust
cargo run
```

The first build compiles Bevy and takes a few minutes.

## Controls

| Key | Action |
| --- | --- |
| WASD / arrow keys | Move the player |
| Space | Shoot |
| P | Pause / resume |
| F1 | Toggle debug mode |
| G / B | Toggle grid / background |
| H / Tab | Toggle hierarchy / inspector |
| Right mouse drag | Pan the editor camera |

## Ideas not built yet

Entity selection and dragging in the viewport, an asset import pipeline, sprite
animation, scripting (Lua, Rhai or WASM), and 3D.

See [CHANGELOG.md](CHANGELOG.md) for the version history.

## License

MIT
