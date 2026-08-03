# Build

`build` compiles and copies XRF source assets into `target/gamedata`. Use it before linking the project into a game
installation, packaging a mod, or validating generated configs/scripts.

```powershell
npm run cli -- build
```

## What it builds

| Target         | Source                                | Output                                                                       |
| -------------- | ------------------------------------- |------------------------------------------------------------------------------|
| `scripts`      | `src/engine/scripts`                  | Lua `.script` files in `target/gamedata/scripts` and related output folders. |
| `externs`      | `src/engine/declarations`             | `src/engine/declarations/extern.json` and `target/gamedata/extern.json`.     |
| `ui`           | `src/engine/forms` plus static UI XML | UI XML under `target/gamedata/configs/ui`.                                   |
| `configs`      | `src/engine/configs`                  | Static and generated config files under `target/gamedata/configs`.           |
| `translations` | `src/engine/translations`             | XML string tables under `target/gamedata/configs/text`.                      |
| `resources`    | configured resource roots             | Static assets copied into `target/gamedata`.                                 |

`target/gamedata/extern.json` is metadata for gamedata verification tools; the game runtime does not load it. The
build also writes `target/gamedata/metadata.json` and stores the build log in `target/xrf_build.log`.

## Options

- `-i, --include <targets...>`: build only selected targets. Choices are `scripts`, `externs`, `ui`, `configs`,
  `translations`, and `resources`.
- `-e, --exclude <targets...>`: skip selected targets. This conflicts with `--include`.
- `-f, --filter <targets...>`: filter copied/generated files by regular-expression strings. Use it only with a specific
  included target, not with the default `all` target set.
- `-l, --language <language>`: use a locale from `cli/config.json`. The default is `ukr`.
- `-c, --clean`: remove `target/gamedata` before building.
- `--nl, --no-lua-logs`: strip Lua logger calls from the compiled script output.
- `--na, --no-asset-overrides`: skip configured override and locale resource roots when copying resources.
- `--itz, --inject-tracy-zones`: inject Tracy profiling zones while compiling scripts.
- `-v, --verbose`: print verbose build logs.

## Examples

```powershell
npm run cli -- build --clean
npm run cli -- build --include scripts configs
npm run cli -- build --include externs
npm run cli -- build --include configs --filter system.ltx
npm run cli -- build --exclude resources
npm run cli -- build --include scripts --no-lua-logs
```

## Failure notes

- Unsupported locales fail before asset copying starts.
- `--filter` with the default `all` include set fails; choose a concrete target first.
- TypeScriptToLua diagnostics fail the script build. Run `npm run typecheck` for a focused error list.
