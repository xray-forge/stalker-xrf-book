# Generate externals documentation

`extern.json` records the Lua-visible externs declared in TypeScript.

## Regenerate

From the engine repository:

```powershell
npm run cli -- build --include externs
```

This writes:

```text
src/engine/declarations/extern.json
target/gamedata/extern.json
```

## Check

```powershell
npm run cli -- verify externs
```

The check compares the tracked JSON with current declarations and does not write files.

For optional HTML or XML references, use [Extern exports](../tools/cli/externs.md).
