# Engine

`engine` manages bundled OpenXRay engine binaries under `cli/bin/engines`. It switches the game installation's `bin`
folder to a selected bundled engine by creating a junction.

```powershell
npm run cli -- engine <command>
```

## Commands

| Command               | Purpose                                                                           |
| --------------------- | --------------------------------------------------------------------------------- |
| `engine list`         | Print available bundled engine names.                                             |
| `engine info`         | Print whether the active game `bin` folder is linked and whether a backup exists. |
| `engine use <engine>` | Switch the configured game installation to a bundled engine.                      |
| `engine rollback`     | Restore the backed-up original `bin` folder.                                      |

## Examples

```powershell
npm run cli -- engine list
npm run cli -- engine use release
npm run cli -- engine info
npm run cli -- engine rollback
```

## How switching works

When `engine use <engine>` sees an unlinked game `bin` folder, it renames it to the XRF backup path and then links the
selected bundled engine. When the current `bin` is already linked, it removes the old link and creates a new one.

`engine rollback` only restores the backup when a backup folder exists and the current `bin` folder is an XRF-linked
engine with `bin.json`.

## Related commands

`pack game --engine <type>` selects a bundled engine for package output without switching the local game installation.
