# Building UI

The UI target writes game UI XML to `target/gamedata/configs/ui` from `src/engine/forms`:

- `.ts` and `.tsx` files that export `create()` are rendered to `.xml` with `renderJsxToXmlText`;
- static `.xml` files are copied as-is;
- `*.test.*` files are ignored by the replication helper.

## Build UI

```powershell
npm run cli -- build --include ui
npm run cli -- build -i ui
```

Use a filter when you only need a subset of files:

```powershell
npm run cli -- build -i ui --filter main_menu
```

Filters are regular-expression strings matched against source file paths.

## Authoring forms

Dynamic forms use JSX-compatible TypeScript. The generated output is XML for the game engine, so form layout still has
to respect X-Ray UI constraints such as absolute coordinates and parent-relative positioning.

Look at existing files in `src/engine/forms` before adding new forms. Reuse shared helpers and components where they
already exist.

## Aspect ratios

X-Ray UI XML often has separate layout expectations for 16:9 and 4:3 modes. Keep generated forms compatible with the
target screen mode and verify in game when changing layout-sensitive XML.
