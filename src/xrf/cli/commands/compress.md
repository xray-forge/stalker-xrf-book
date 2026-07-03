# Compress

`compress` packs built `target/gamedata` content into database archives with `xrCompress`. Use it after `build` when you
need DB archives for a game package.

```powershell
npm run cli -- compress
```

## Targets

Compression target definitions live in `cli/compress/configs/compress.json`.

| Target      | Packed content                                       |
| ----------- | ---------------------------------------------------- |
| `configs`   | `configs`, `spawns`, `anims`, and `ai`.              |
| `levels`    | `levels`.                                            |
| `resources` | `textures` and `meshes`.                             |
| `shaders`   | shader-related `.xr` files and the `shaders` folder. |
| `sounds`    | `sounds`, stored without compression.                |

Output archives are written under `target/db` as `<target>.dbN`.

## Options

- `-i, --include <targets...>`: compress selected targets. Defaults to `all`.
- `-c, --clean`: remove `target/db` before writing archives.
- `-v, --verbose`: print `xrCompress` output.

## Examples

```powershell
npm run cli -- build --clean
npm run cli -- compress --clean
npm run cli -- compress --include configs shaders
```

## Inputs and output

The command reads built files from `target/gamedata` and writes database archives under `target/db`. It does not build
missing gamedata by itself, so run `build` first when source files changed.

Use `--include` for fast package checks when only one archive group changed. Use `--clean` before producing a package
that should not contain stale archives from a previous target set.

## Failure notes

`target/gamedata` must exist. If compression target names are wrong, the command prints the valid names from the
compression config.
