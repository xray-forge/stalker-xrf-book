# Building Configs

The configs build target writes LTX and XML files to `target/gamedata/configs`.

It processes sources from `src/engine/configs`:

- dynamic `.ts` files export `create()` or `config` and render to `.ltx`;
- dynamic `.tsx` files export `create()` and render to `.xml`;
- static `.ltx` and `.xml` files are copied as-is;
- `*.test.*` files are ignored.

## Build Only Configs

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

## Dynamic LTX

Dynamic LTX configs use structured TypeScript descriptors and `renderJsonToLtx`. This is useful when a config needs
shared constants, loops, generated sections, or tests.

Static LTX is still appropriate for simple files that do not need build-time logic.

## Dynamic XML

Dynamic XML configs use JSX and `renderJsxToXmlText`. They are separate from UI forms: config XML is built from
`src/engine/configs`, while UI XML is built from `src/engine/forms`.

## Validation

Use the LTX verifier after config changes:

```powershell
npm run cli -- verify ltx
```

Strict mode is available for stricter checks:

```powershell
npm run cli -- verify ltx --strict
```
