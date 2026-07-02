# Open

Open commands launch the system file explorer for common project and game folders.

```powershell
npm run cli -- open_game_folder
npm run cli -- open_project_folder
```

## Commands

| Command               | Opens                                                |
| --------------------- | ---------------------------------------------------- |
| `open_game_folder`    | The configured or detected S.T.A.L.K.E.R. game root. |
| `open_project_folder` | The `stalker-xrf-engine` repository root.            |

`open_game_folder` uses the same game path resolution as `link`, `logs`, `start_game`, and `verify project`.

## Examples

```powershell
npm run cli -- open_game_folder
npm run cli -- open_project_folder
```

## Failure notes

If the game folder does not open, check `cli/config.json` under `targets` or run `npm run cli -- verify project` to see
which path is being resolved.
