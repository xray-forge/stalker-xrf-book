# Gamedata CLI

Gamedata commands validate a project across one or more gamedata roots. Use them before packaging or when investigating
missing textures, meshes, animations, scripts, configs, particles, or weather data.

## `verify-gamedata`

```powershell
xrf-tool verify-gamedata --root ./gamedata --configs ./gamedata/configs
```

## Options

- `-r, --root <paths...>`: one or more gamedata roots. Required. Multiple paths are comma-separated.
- `-i, --ignore <names...>`: ignored files or folders. Multiple names are comma-separated.
- `-c, --configs <path>`: configs folder. Defaults to `configs` under the first root.
- `--checks <checks...>`: selected verification checks. If omitted, all checks run.
- `--silent`: disable logging.
- `-v, --verbose`: enable verbose logging.
- `-s, --strict`: enable strict mode.

If `--ignore` is omitted, the command ignores common repository and unpacked-source entries: `.git`, `.idea`,
`particles_unpacked`, `textures_unpacked`, `.gitignore`, `.gitattributes`, `README.md`, and `LICENSE`.

## Examples

```powershell
xrf-tool verify-gamedata --root ./gamedata
xrf-tool verify-gamedata --root ./base,./override --configs ./override/configs
xrf-tool verify-gamedata --root ./gamedata --ignore .git,textures_unpacked --strict
```

## Result

The command exits with a non-zero status when the project is invalid. In normal logging mode it prints each failure
message before exiting.
