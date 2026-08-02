# Extensions

Extensions are optional modules discovered in `gamedata/extensions`. They keep gameplay changes separate from the core
script layer.

At game startup, XRF scans folders containing `main.script`, restores their saved order and enabled state, and registers
enabled modules.

## Extension Entry Point

Each extension has its own folder under `extensions` and an entry file named `main.script` after build output.

An extension module can export:

- `register(isNewGame, extension)`: called when the extension is enabled; this is required for a usable module.
- `unregister(isNewGame, extension)`: optional cleanup hook called when the extension is disabled.
- `save(data)`: optional hook for extension dynamic data.
- `load(data)`: optional hook for restoring extension dynamic data.

The TypeScript sources for built-in extensions live under `src/engine/extensions`.

## State and ordering

Loaded extensions are stored in the runtime registry by name. The load order and enabled state are saved to
`extensions_order.scopo` in the game saves folder.

The main menu can reorder extensions and toggle those that opt in with `canToggle`.

## Built-In Extensions

The current engine source includes these extension folders:

| Source folder                   | Extension name                        | Default state |
| ------------------------------- | ------------------------------------- | ------------- |
| `achievements_rewards`          | `Achievement rewards`                 | enabled       |
| `enhanced_items_drop`           | `Enhanced items drop (with upgrades)` | disabled      |
| `enhanced_location_progression` | `Enhanced location progression`       | enabled       |
| `enhanced_treasures`            | `Enhanced treasures`                  | enabled       |
| `original_start_position`       | `Original start position`             | disabled      |

The built-in modules are useful references when adding an extension:

- [Achievement rewards](extensions/achievement_rewards.md)
- [Enhanced items drop](extensions/enhanced_items_drop.md)
- [Enhanced location progression](extensions/enhanced_location_progression.md)
- [Enhanced treasures](extensions/enhanced_treasures.md)
- [Original start position](extensions/original_start_position.md)

## Config Files

Extensions can open extension-local LTX files through the extension utilities. `main.ltx` is the default relative file
name when no file name is provided.

## Scope

XRF supports discovery, ordering, enablement, registry registration, and save/load hooks. It does not currently define
extension dependencies, packaging metadata, or extension-specific build steps.
