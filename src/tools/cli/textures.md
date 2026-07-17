# Texture CLI

Texture commands inspect DDS files and pack or unpack icon-related assets.

## Commands

| Command                      | Purpose                                                                    |
| ---------------------------- | -------------------------------------------------------------------------- |
| `info-dds`                   | Print DDS size, metadata, mipmap, format, and compression details.         |
| `unpack-equipment-icons`     | Slice an equipment icon sprite into section icon files using `system.ltx`. |
| `pack-equipment-icons`       | Pack section icon files into an equipment DDS sprite using `system.ltx`.   |
| `unpack-texture-description` | Slice textures based on XML texture descriptions.                          |
| `pack-texture-description`   | Pack textures based on XML texture descriptions.                           |

## DDS inspection

```powershell
xrf-cli info-dds --path ./textures/ui/ui_icon_equipment.dds
```

The command prints file size, metadata size, pixel data size, dimensions, mipmap information, pitch or linear size when
present, block size, bits per pixel, FourCC, and D3D/DXGI format when known.

## Equipment icons

```powershell
xrf-cli unpack-equipment-icons --system-ltx ./configs/system.ltx --source ./textures/ui/ui_icon_equipment.dds --output ./textures_unpacked/ui/ui_icon_equipment
xrf-cli pack-equipment-icons --system-ltx ./configs/system.ltx --source ./textures_unpacked/ui/ui_icon_equipment --output ./textures/ui/ui_icon_equipment.dds --strict
```

`pack-equipment-icons` also accepts `--gamedata <path>` for resource lookup, plus `-v, --verbose` and `-s, --strict`.
`unpack-equipment-icons` supports `-v, --verbose`.

## Texture descriptions

```powershell
xrf-cli unpack-texture-description --description ./configs/ui/textures_descr/ui_actor.xml --base ./textures --output ./textures_unpacked --parallel
xrf-cli pack-texture-description --description ./configs/ui/textures_descr/ui_actor.xml --base ./textures_unpacked --output ./textures --strict
```

Description commands require `--description` and `--base`. If `--output` is omitted, output defaults to the base path.
Both support `-v, --verbose`, `-s, --strict`, and `--parallel`.

The engine repository wraps the common equipment and description workflows through `npm run cli -- icons ...`.
