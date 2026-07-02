# Parse

`parse` contains utility commands that generate JSON or HTML support files under `target/parsed`.

```powershell
npm run cli -- parse <command>
```

## Commands

| Command                    | Purpose                                                                     | Output                         |
| -------------------------- | --------------------------------------------------------------------------- | ------------------------------ |
| `parse dir_as_json <path>` | Flatten a directory tree into a JSON object keyed by normalized file names. | `target/parsed/<folder>.json`  |
| `parse externals`          | Render script declaration externs into a small HTML reference.              | `target/parsed/externals.html` |

`dir_as_json` resolves `<path>` relative to the repository root.

## Options

`parse dir_as_json` supports:

- `-e, --no-extension`: omit file extensions in JSON values.

## Examples

```powershell
npm run cli -- parse dir_as_json src/resources/textures
npm run cli -- parse dir_as_json src/resources/textures --no-extension
npm run cli -- parse externals
```

## Failure notes

`dir_as_json` requires a path argument. `parse externals` reads TypeScript declaration sources under
`src/engine/scripts/declarations` and skips tests and index files.
