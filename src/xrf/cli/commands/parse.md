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

## Output usage

Use `dir_as_json` when another script needs a compact index of files under a resource folder. The command writes
generated support data under `target/parsed`, so treat the result as disposable build output.

Use `parse externals` when checking the script declaration surface exposed by conditions, effects, and dialogs. The
generated HTML is a local reference; edit the TypeScript declaration sources to change the exported behavior.

## Failure notes

`dir_as_json` requires a path argument. `parse externals` reads TypeScript declaration sources under
`src/engine/scripts/declarations` and skips tests and index files.
