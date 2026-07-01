# Building

The build command compiles and copies XRF source files into X-Ray-compatible `gamedata` output under `target/gamedata`.

```powershell
npm run build
npm run cli -- build
```

## Build Targets

The build target names are:

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
- `-f, --filter <targets...>`: filter source files by regular-expression strings.
- `-c, --clean`: remove `target/gamedata` before building.
- `--nl, --no-lua-logs`: strip Lua logger calls from the compiled script output.
- `--na, --no-asset-overrides`: skip configured override and locale resource roots.
- `--itz, --inject-tracy-zones`: inject Tracy profiling zones into compiled scripts.

Filters must be used with an explicit `--include` target. The build command rejects filtered implicit `all` builds.

## Build Steps

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

The command writes generated gamedata to `target/gamedata` and collects a build log in `target/xrf_build.log`.

Do not edit `target/gamedata` by hand. Change the source file and rebuild.
