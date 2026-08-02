# Building Configs

Build configs into `target/gamedata/configs` from `src/engine/configs`.

The target processes:

- dynamic `.ts` files export `create()` or `config` and render to `.ltx`;
- dynamic `.tsx` files export `create()` and render to `.xml`;
- static `.ltx` and `.xml` files are copied as-is;
- `*.test.*` files are ignored.

## Build configs

```powershell
npm run cli -- build --include configs
npm run cli -- build -i configs
```

Use filters for focused rebuilds:

```powershell
npm run cli -- build -i configs --filter system.ltx
```

Filters are regular-expression strings matched against source file paths. The build command does not allow filters with
the implicit `all` target, so combine `--filter` with `--include`.

## Source types

Dynamic LTX configs use structured TypeScript descriptors and `renderJsonToLtx`. This is useful when a config needs
shared constants, loops, generated sections, or tests.

Static LTX is still appropriate for simple files that do not need build-time logic.

Dynamic XML configs use JSX and `renderJsxToXmlText`. UI XML is separate: it is built from `src/engine/forms`.

## Validation

Use the LTX verifier after config changes:

```powershell
npm run cli -- verify ltx
```

Strict mode is available for stricter checks:

```powershell
npm run cli -- verify ltx --strict
```
