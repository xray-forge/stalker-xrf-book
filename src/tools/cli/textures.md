# Texture CLI

Texture commands inspect DDS files and pack or unpack icon-related assets.

## Commands

| Command                      | Purpose                                                                    |
| ---------------------------- | -------------------------------------------------------------------------- |
| `info-dds`                   | Print DDS size, metadata, mipmap, format, and compression details.         |
| `crop-dds`                   | Crop a pixel region out of a DDS file into a new DDS or PNG file.          |
| `unpack-equipment-icons`     | Slice an equipment icon sprite into section icon files using `system.ltx`. |
| `pack-equipment-icons`       | Pack section icon files into an equipment DDS sprite using `system.ltx`.   |
| `verify-equipment-icons`     | Report inventory icon grid rects that overlap each other.                  |
| `unpack-texture-description` | Slice textures based on XML texture descriptions.                          |
| `pack-texture-description`   | Pack textures based on XML texture descriptions.                           |

## DDS inspection

```powershell
xrf-cli info-dds --path ./textures/ui/ui_icon_equipment.dds
```

The command prints file size, metadata size, pixel data size, dimensions, mipmap information, pitch or linear size when
present, block size, bits per pixel, FourCC, and D3D/DXGI format when known.

## Region cropping

```powershell
xrf-cli crop-dds --source ./textures/ui/ui_icon_equipment.dds --output ./wpn_ak74.dds --x 1000 --y 0 --width 250 --height 100
```

`--source`, `--output`, `--x`, `--y`, `--width`, and `--height` are required. Coordinates start at the top left. An
out-of-bounds region is rejected rather than truncated. The command also accepts `-s, --silent` and `-v, --verbose`.

The `.png` extension selects lossless PNG; any other name produces a BC3 DDS. Prefer PNG as a later packing source to
avoid another lossy decode and encode. `pack-equipment-icons` prefers `<section>.png` over `<section>.dds`.

Use this to lift a single icon out of a sprite sheet when the sheet belongs to another project, where
`unpack-equipment-icons` cannot help because that project's configs do not declare `$inventory_icon`.

### Fitting into different bounds

`--fit-width` and `--fit-height` scale the cropped region after cropping. They must be given together.

```powershell
xrf-cli crop-dds --source ./ui_actor_weapons.dds --output ./upgrade_ak74.dds --x 0 --y 400 --width 300 --height 100 --fit-width 295 --fit-height 110
```

Scaling preserves aspect ratio and centers the result on a transparent canvas instead of stretching it. A matching
region is unchanged. `pack-equipment-icons` uses the same fitting for mismatched icons.

Fitting matters where a target rectangle is fixed by an XML texture description, since `pack-texture-description`
requires the file to match the declared size exactly and does not rescale.

## Equipment icons

```powershell
xrf-cli unpack-equipment-icons --system-ltx ./configs/system.ltx --source ./textures/ui/ui_icon_equipment.dds --output ./textures_unpacked/ui/ui_icon_equipment
xrf-cli pack-equipment-icons --system-ltx ./configs/system.ltx --source ./textures_unpacked/ui/ui_icon_equipment --output ./textures/ui/ui_icon_equipment.dds --strict
```

`pack-equipment-icons` also accepts `--gamedata <path>` for resource lookup, plus `-v, --verbose` and `-s, --strict`.
`unpack-equipment-icons` supports `-v, --verbose`.

Both commands act only on sections with `$inventory_icon = true` and all four grid fields: `inv_grid_x`, `inv_grid_y`,
`inv_grid_width`, and `inv_grid_height`. Grid fields alone do not opt a section in; `$inventory_icon = false` excludes
it.

The explicit flag matters because inherited grid fields are common on abstract base sections that have no icon of their
own.

Without `--strict`, an opted-in section whose icon file is missing is skipped with a warning. With `--strict` the
command packs nothing and reports every such section in a single error, so the whole list can be fixed in one pass.

### Checking the grid before moving an icon

```powershell
xrf-cli verify-equipment-icons --system-ltx ./configs/system.ltx
```

Reports every pair of sections whose grid rects cover a shared cell, with the cell and how many cells overlap, and exits
non-zero when any are found.

This covers a case packing cannot. `pack-equipment-icons` warns when two sections write different art to the **same**
slot, which is the common and usually harmless case, because variants such as `_nimble`, `_snag` and the `pri_a15_`
quest copies inherit their base weapon's position by design. Identical rects are therefore not reported here either.

What it catches instead is a rect reaching **into** a neighbour: widen a `1x1` icon to `2x1` and it may quietly take a
cell another weapon already occupies. Both pack without complaint, and whichever writes last wins those pixels, so the
loser silently shows the wrong art. Run this before widening or relocating any icon, and again afterwards.

Grid coordinates are cells, not pixels; a cell is 50x50, hardcoded in the engine.

## Texture descriptions

```powershell
xrf-cli unpack-texture-description --description ./configs/ui/textures_descr/ui_actor.xml --base ./textures --output ./textures_unpacked --parallel
xrf-cli pack-texture-description --description ./configs/ui/textures_descr/ui_actor.xml --base ./textures_unpacked --output ./textures --strict
```

Description commands require `--description` and `--base`. If `--output` is omitted, output defaults to the base path.
Both support `-v, --verbose`, `-s, --strict`, and `--parallel`.

A description can name several sheets; both commands rewrite all of them by default. Repeat `--file <name>` to select
one or more. Use the declared path (`ui\ui_actor_weapons`) with either separator, or an unambiguous bare name
(`ui_actor_weapons`). Missing and ambiguous names are errors.

```powershell
xrf-cli pack-texture-description --description ./configs/ui/textures_descr/ui_actor_upgrades.xml --base ./textures_unpacked --output ./textures --file ui_actor_weapons --strict
```

The engine repository wraps the common equipment and description workflows through `npm run cli -- icons ...`.
