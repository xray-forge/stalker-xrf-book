# Icons

`icons` wraps texture tooling from `cli/bin/tools/xrf-cli`. It packs and unpacks equipment icon sprites and UI texture
description sprites using project paths from `cli/config.json`.

```powershell
npm run cli -- icons <command>
```

## Commands

| Command                     | Reads                                                                                 | Writes                                                 |
| --------------------------- | ------------------------------------------------------------------------------------- | ------------------------------------------------------ |
| `icons unpack-equipment`    | `src/resources/textures/ui/ui_icon_equipment.dds` and `src/engine/configs/system.ltx` | `src/resources/textures_unpacked/ui/ui_icon_equipment` |
| `icons pack-equipment`      | unpacked equipment icons and `system.ltx`                                             | `src/resources/textures/ui/ui_icon_equipment.dds`      |
| `icons unpack-descriptions` | UI texture descriptions and packed textures                                           | `src/resources/textures_unpacked`                      |
| `icons pack-descriptions`   | UI texture descriptions and unpacked textures                                         | `src/resources/textures`                               |

## Options

All icon commands support:

- `-v, --verbose`: print verbose logs.
- `-s, --strict`: enable strict mode.

Description commands also support:

- `-d, --description <name>`: process one file under `src/engine/forms/textures_descr`.

## Examples

```powershell
npm run cli -- icons unpack-equipment
npm run cli -- icons pack-equipment --strict
npm run cli -- icons unpack-descriptions --description ui_actor.xml
npm run cli -- icons pack-descriptions --description ui_actor.xml
```

## Workflow

Unpack before editing icon sprites or checking generated sprite coordinates. Pack after editing the unpacked files or
texture descriptions. Use `--description` when working on a single UI texture description file instead of the whole
description set.

Equipment commands are tied to the equipment icon atlas. Description commands are tied to XML texture description files
under `src/engine/forms/textures_descr`.

## Failure notes

Equipment commands depend on valid `system.ltx` icon coordinates. Description commands depend on XML description names
and matching source textures.
