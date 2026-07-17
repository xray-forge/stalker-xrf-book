# Icon Editor

The icon editor works with equipment icon sprites and icon descriptor data. The fully wired workflow is equipment sprite
opening and equipment icon packing.

## Screens

| Screen             | Route                                    | Status                                                                      |
| ------------------ | ---------------------------------------- | --------------------------------------------------------------------------- |
| Equipment editor   | `/icons_editor/icons_equipment`          | Opens a DDS equipment sprite with `system.ltx` descriptors and displays it. |
| Equipment pack     | `/icons_editor/icons_equipment_pack`     | Packs separate icon files into an equipment DDS sprite.                     |
| Equipment unpack   | `/icons_editor/icons_equipment_unpack`   | Route and form shell. No backend unpack command is wired in the current UI. |
| Description editor | `/icons_editor/icons_description`        | Route shell.                                                                |
| Description pack   | `/icons_editor/icons_description_pack`   | Route and form shell.                                                       |
| Description unpack | `/icons_editor/icons_description_unpack` | Route and form shell.                                                       |

## Backend commands

The Tauri `icons-editor` plugin exposes:

- `open_equipment_sprite`;
- `reopen_equipment_sprite`;
- `get_equipment_sprite`;
- `close_equipment_sprite`;
- `pack_equipment`.

`open_equipment_sprite` reads the DDS as a PNG preview and reads equipment descriptors from `system.ltx`.
`pack_equipment` packs source icons into a DDS using BC3 RGBA compression.

## Workflow

1. Open the equipment sprite with the matching `system.ltx`.
2. Inspect section icon rectangles in the equipment editor.
3. Use **Equipment pack** when separate icon files should be packed into the final equipment DDS.

When an XRF project path is configured, the app tries to prefill common paths such as `src/engine/configs/system.ltx`
and `src/resources/textures/ui/ui_icon_equipment.dds`.

## CLI equivalent

Use `xrf-cli unpack-equipment-icons`, `xrf-cli pack-equipment-icons`, `xrf-cli unpack-texture-description`, and
`xrf-cli pack-texture-description` when you need workflows that are not fully wired in the app.
