# Building

Build XRF into `target/gamedata` before testing, linking, or packaging.

```powershell
npm run build
npm run cli -- build
```

## Targets

`build` runs every target by default:

- `scripts`
- `ui`
- `configs`
- `translations`
- `resources`

By default, `build` runs all targets. Use `--include` for focused builds and `--exclude` to skip targets:

```powershell
npm run cli -- build --include scripts configs
npm run cli -- build --exclude resources
```

## Options

- `-i, --include <targets...>`: build only selected targets.
- `-e, --exclude <targets...>`: build all targets except selected targets.
- `-v, --verbose`: print verbose build logs.
- `-l, --language <language>`: build with a locale from `cli/config.json`.
- `-f, --filter <patterns...>`: regular-expression filters for source paths.
- `-c, --clean`: remove `target/gamedata` before building.
- `--nl, --no-lua-logs`: strip Lua logger calls from the compiled script output.
- `--na, --no-asset-overrides`: skip configured override and locale resource roots.
- `--itz, --inject-tracy-zones`: inject Tracy profiling zones into compiled scripts.

`--filter` requires an explicit `--include`; it cannot be used with the default all-target build.

## What a full build does

When all targets run, the build performs these steps:

1. optionally clean `target/gamedata`;
2. compile TypeScript scripts to Lua;
3. render dynamic UI forms and copy static UI XML;
4. render dynamic configs and copy static LTX/XML configs;
5. build translations;
6. copy static resources;
7. write `metadata.json`;
8. collect the build log.

## Examples

```powershell
npm run cli -- build
npm run cli -- build --clean
npm run cli -- build --include ui
npm run cli -- build --include configs --filter system.ltx
npm run cli -- build --exclude resources
npm run cli -- build --no-lua-logs --inject-tracy-zones
```

## Output

The command writes `target/gamedata`, `target/gamedata/metadata.json`, and `target/xrf_build.log`. Change source files
and rebuild rather than editing generated output.
