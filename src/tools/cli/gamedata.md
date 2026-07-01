# Gamedata CLI

Gamedata commands validate a gamedata project across one or more roots.

## `verify-gamedata`

```powershell
xrf-tool verify-gamedata --root ./gamedata --configs ./gamedata/configs
```

Options:

- `-r, --root <paths...>`: one or more gamedata roots. Required.
- `-i, --ignore <names...>`: ignored files or folders.
- `-c, --configs <path>`: configs folder. Defaults to `configs` under the first root.
- `--checks <checks...>`: selected check types.
- `--silent`: disable logging.
- `-v, --verbose`: enable verbose logging.
- `-s, --strict`: enable strict mode.

If no ignored entries are provided, the command ignores common repository and unpacked-source files such as `.git`,
`.idea`, `particles_unpacked`, `textures_unpacked`, `README.md`, and `LICENSE`.
