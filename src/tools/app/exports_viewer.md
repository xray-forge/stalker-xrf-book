# Exports Viewer

The exports viewer reads declaration folders and shows exported conditions, dialog functions, effects, and parameters.

Use it when editing config logic and you need to confirm which script externs are available.

## Screens

| Screen       | Route                     | Purpose                                                      |
| ------------ | ------------------------- | ------------------------------------------------------------ |
| Navigator    | `/exports_editor`         | Open export sources.                                         |
| Exports view | `/exports_editor/exports` | Browse parsed declarations and filter the displayed exports. |

## Backend commands

The Tauri `exports-editor` plugin exposes:

- `open_xr_exports`: parse conditions, dialogs, and effects folders together;
- `get_xr_exports`;
- `close_xr_exports`;
- `open_xr_effects`;
- `parse_xr_effects`;
- `get_xr_effects`;
- `close_xr_effects`;
- `has_xr_effects`.

## Source folders

When an XRF project path is configured, the UI can derive default declaration paths from:

- `src/engine/scripts/declarations/conditions`;
- `src/engine/scripts/declarations/dialogs`;
- `src/engine/scripts/declarations/effects`.

## Notes

The viewer is read-only. Edit the engine declaration sources, then reload the viewer.

Use the CLI-generated externs reference when you need a static artifact under `target/parsed`. Use the app viewer when
you are browsing declarations interactively while editing configs.

If a condition or effect is missing from the viewer, check that it is declared in the expected conditions, dialogs, or
effects folder and that the project path points at the engine repository you are editing.
