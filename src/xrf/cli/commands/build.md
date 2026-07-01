# Build

Builds XRF source files into `target/gamedata`.

```powershell
npm run cli -- build
```

## Options

- `-i, --include <targets...>`: build only selected targets.
- `-e, --exclude <targets...>`: exclude selected targets.
- `-v, --verbose`: print verbose logs.
- `-l, --language <language>`: use a locale from `cli/config.json`.
- `-f, --filter <targets...>`: filter source files by regular-expression strings.
- `-c, --clean`: remove `target/gamedata` before building.
- `--nl, --no-lua-logs`: strip Lua logger calls from script output.
- `--na, --no-asset-overrides`: skip configured override and locale asset roots.
- `--itz, --inject-tracy-zones`: inject Tracy profiling zones into compiled scripts.

## Examples

```powershell
npm run cli -- build --clean
npm run cli -- build --include scripts configs
npm run cli -- build --include configs --filter system.ltx
npm run cli -- build --exclude resources
```
