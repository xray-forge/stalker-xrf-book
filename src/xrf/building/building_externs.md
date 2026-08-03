# Building the extern manifest

Build externs after changing an `extern(...)` declaration or its JSDoc.

```powershell
npm run cli -- build --include externs
```

## Output

- `src/engine/declarations/extern.json` — tracked contract.
- `target/gamedata/extern.json` — packaged sidecar for gamedata tools.

The game does not load either file. Do not edit them by hand. Manifest source paths are relative to the declarations root.

## Check the tracked manifest

```powershell
npm run cli -- verify externs
```

This compares declarations with the tracked manifest without writing. Regenerate when it reports drift.

## Requirements

Callable externs need explicit parameter and return annotations. Values need `value as Type`. The build fails for
duplicates, dynamic names, and unsupported declaration shapes.
