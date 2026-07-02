# Start

`start_game` starts the configured game executable.

```powershell
npm run cli -- start_game
```

The executable path is resolved from the same game-path logic used by `link`, `open_game_folder`, `logs`, and
`verify project`. Configuration lives in `cli/config.json` under `targets`:

- `stalker_game_steam_id`: Steam app id used for automatic detection;
- `stalker_game_fallback_path`: fallback game folder when Steam detection is not enough;
- `stalker_app_path`: executable name inside the game folder.

## Typical workflow

```powershell
npm run cli -- build --clean
npm run cli -- link
npm run cli -- start_game
```

## Failure notes

If the process does not start, run `npm run cli -- verify project` and check the resolved game folder and executable.
The command starts the executable asynchronously, so later runtime errors are written to the engine log rather than to
the CLI process.
