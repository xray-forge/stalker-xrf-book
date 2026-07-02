# Command line arguments

Command line arguments are read by the engine before scripts are loaded. Use them for engine startup choices such as
filesystem layout, renderer, logs, game mode, Lua JIT, and initial level loading.

`npm run cli -- start_game` starts the configured game executable. It does not inject arbitrary engine arguments for
you. To pass arguments, run the executable directly, configure a launcher shortcut, or extend the local start command.

## Common flags

| Flag                  | Effect                                                                              |
| --------------------- | ----------------------------------------------------------------------------------- |
| `-fsltx <file>`       | Use a specific filesystem configuration file before the engine core is initialized. |
| `-ltx <file>`         | Use a specific console/user configuration file instead of the default `user.ltx`.   |
| `-start <args>`       | Execute a `start ...` console command after engine initialization.                  |
| `-load <save>`        | Execute a `load ...` console command after engine initialization.                   |
| `-nointro`            | Skip intro playback.                                                                |
| `-nogameintro`        | Skip the in-game intro sequence.                                                    |
| `-nosplash`           | Disable the startup splash screen.                                                  |
| `-splashnotop`        | Show the splash screen without forcing it on top.                                   |
| `-dedicated`          | Start in dedicated server mode.                                                     |
| `-i`                  | Disable input capture used by the normal game window.                               |
| `-overlaypath <path>` | Override the app data/logs root used by the engine locator.                         |
| `-nolog`              | Do not create the main log file.                                                    |
| `-unique_logs`        | Write logs with unique timestamped names.                                           |
| `-force_flushlog`     | Flush log output aggressively. Useful when debugging crashes.                       |
| `-nojit`              | Disable LuaJIT JIT compilation. This also changes profiler behavior.                |
| `-dump_bindings`      | Dump script binding information from the Lua script engine.                         |

Renderer flags such as `-r1`, `-r2`, `-r2a`, `-r2.5`, `-r3`, `-r4`, and `-rgl` are engine-build dependent. Verify the
selected executable before documenting a renderer as supported for a pack.

## Game mode flags

OpenXRay-style builds can select a compatibility mode from the command line:

- `-cop` for Call of Pripyat mode;
- `-cs` for Clear Sky mode;
- `-shoc` or `-soc` for Shadow of Chernobyl mode;
- `-unlock_game_mode` to allow explicit game mode selection.

If no mode is selected, the engine can fall back to `openxray.ltx` compatibility settings.

## Examples

Start with an explicit filesystem config and user config:

```bat
xrEngine.exe -fsltx fsgame.ltx -ltx user.ltx -nointro
```

Start a new local game through the engine console startup command:

```bat
xrEngine.exe -start "server(all/single/alife/new) client(localhost)"
```

Load a save after initialization:

```bat
xrEngine.exe -load quicksave
```

Quote values that contain spaces. Treat command line support as executable-specific: forks can add, remove, or rename
flags.
