# Building the extern manifest

The `externs` target records XRF Lua-visible exports declared in `src/engine/declarations`. Run it after changing an
`extern(...)` declaration or its JSDoc.

```powershell
npm run cli -- build --include externs
```

## Output

The build writes the same generated JSON to two locations:

- `src/engine/declarations/extern.json` is the committed source contract.
- `target/gamedata/extern.json` is the packaged sidecar for gamedata verification tools.

Each export is keyed by its Lua-visible name. Entries record the declaration source path, parameter and return types for
callables, value types for data exports, and available JSDoc descriptions.

The game runtime does not load this file. Do not edit either generated copy by hand.

## Build failures

The target fails when two declarations use the same Lua-visible name or when an `extern(...)` declaration cannot be
represented statically. Fix the declaration before rebuilding.
