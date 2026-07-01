# Extensions

Extensions are optional script modules loaded from `gamedata/extensions`. They let XRF keep larger gameplay changes
separate from the core script engine.

The engine scans extension folders, loads each `main.script`, synchronizes the list with saved user preferences, and
registers enabled extensions during game start.

## Extension Entry Point

Each extension has its own folder under `extensions` and an entry file named `main.script` after build output.

The module can export:

- `register(isNewGame, extension)`: called when the extension is enabled.
- `unregister(isNewGame, extension)`: optional cleanup hook called when the extension is disabled.
- `save(data)`: optional hook for extension dynamic data.
- `load(data)`: optional hook for restoring extension dynamic data.

The TypeScript sources for built-in extensions live under `src/engine/extensions`.

## Runtime State

Loaded extensions are stored in the runtime registry by name. The load order and enabled state are saved to
`extensions_order.scopo` in the game saves folder.

The main menu includes an extensions dialog. It lets the user reorder extensions and toggle extensions that declare they
can be toggled.

## Built-In Extensions

The current engine source includes these extension folders:

- `achievements_rewards`
- `enhanced_items_drop`
- `enhanced_location_progression`
- `enhanced_treasures`
- `original_start_position`

Use these as implementation references when adding a new extension.

## Config Files

Extensions can open extension-local LTX files through the extension utilities. `main.ltx` is the default relative file
name when no file name is provided.

## Limitations

The current implementation covers discovery, ordering, enabling/disabling, registry registration, and save/load hooks.
More advanced extension packaging, dependency declarations, and extension-specific build steps should be treated as
future work unless the source adds them.
