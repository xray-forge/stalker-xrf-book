# Building Scripts

The scripts build target compiles TypeScript under `src/engine/scripts` and related imported engine code to Lua with
TypeScriptToLua.

The output is written into `target/gamedata` as X-Ray script files. The generated bundle also includes
`lualib_bundle.script`, which provides helper functions emitted by TypeScriptToLua.

## Build Only Scripts

```powershell
npm run cli -- build --include scripts
npm run cli -- build -i scripts
```

Use `--no-lua-logs` when you want compiled scripts without Lua logger calls:

```powershell
npm run cli -- build -i scripts --no-lua-logs
```

Use `--inject-tracy-zones` when profiling with Tracy support:

```powershell
npm run cli -- build -i scripts --inject-tracy-zones
```

## Watch Mode

Use watch mode during script development:

```powershell
npm run watch:scripts
```

Additional script watch commands are available for optimized and Tracy-instrumented builds:

```powershell
npm run watch:scripts-optimized
npm run watch:scripts-tracy
npm run watch:scripts-tracy-optimized
```

## Type Checking

Run TypeScriptToLua type checking without emitting files:

```powershell
npm run typecheck
```

When script compilation reports diagnostics, use `typecheck` for a focused failure report.

## Type Definitions

X-Ray engine APIs are exposed through TypeScript declarations from the `xray16` package and the XRF typedefs under
`src/typedefs`. Use the generated type documentation when checking engine class, method, and enum names:

- [xray-16-types source](https://github.com/xray-forge/xray-16-types)
- [xray-16-types documentation](https://xray-forge.github.io/xray-16-types/index.html)

## Luabind Classes

XRF uses custom TypeScriptToLua transforms for luabind-style classes. Classes that need engine-compatible luabind
registration use project decorators and generated Lua class shapes instead of plain TypeScriptToLua metatable classes.
