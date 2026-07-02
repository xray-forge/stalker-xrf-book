# Link

`link`, `unlink`, and `relink` connect the build output and game folders used during local development.

```powershell
npm run cli -- link
```

## What gets linked

| Link          | Source                      | Destination                       |
| ------------- | --------------------------- | --------------------------------- |
| Game link     | configured game root        | `target/game_link`                |
| Gamedata link | `target/gamedata`           | configured game `gamedata` folder |
| Logs link     | configured game logs folder | `target/logs_link`                |

Game paths come from Steam detection or the fallback values in `cli/config.json`.

## Commands

- `link`: create the game, gamedata, and logs links.
- `unlink`: remove the gamedata link, logs link, and game link.
- `relink`: run `unlink`, then `link`.

`link` and `relink` support `-f, --force` to remove existing link targets first. Use it carefully: if a real game
`gamedata` folder exists, `--force` removes it before creating the link.

## Examples

```powershell
npm run cli -- build --clean
npm run cli -- link
npm run cli -- relink --force
npm run cli -- unlink
```

## Failure notes

If linking fails, verify the configured game path, executable name, and Steam installation detection. `verify project`
checks the same paths.
