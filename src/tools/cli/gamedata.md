# Gamedata CLI

Gamedata commands validate one assembled gamedata directory. Run them after building gamedata and before launching or
packaging it.

## `verify-gamedata`

```powershell
xrf-cli verify-gamedata ./target/gamedata
```

`ROOT` is the required positional path to the assembled gamedata directory. The command reads configs from
`ROOT/configs` and requires `ROOT/configs/system.ltx`.

## Options

- `-i, --ignore <names...>`: ignored files or folders. Multiple names are comma-separated.
- `--checks <checks...>`: selected verification checks. If omitted, all checks run.
- `--silent`: disable logging.
- `-v, --verbose`: enable verbose logging.
- `-s, --strict`: enable strict mode.

Accepted check names are `animations`, `levels`, `ltx`, `meshes`, `particles`, `particles-usage`, `scripts`, `shaders`,
`sounds`, `spawns`, `textures`, `weapons`, and `weathers`. The script check parses emitted `.script` files with the
LuaJIT syntax dialect.

If `--ignore` is omitted, the command ignores common repository and unpacked-source entries: `.git`, `.idea`,
`particles_unpacked`, `textures_unpacked`, `.gitignore`, `.gitattributes`, `README.md`, and `LICENSE`.

## Examples

```powershell
xrf-cli verify-gamedata ./target/gamedata
xrf-cli verify-gamedata ./target/gamedata --checks scripts,ltx
xrf-cli verify-gamedata ./target/gamedata --ignore .git,textures_unpacked --strict
```

## Result

The command exits with a non-zero status when the assembled gamedata is invalid or its required root/config layout is
missing. In normal logging mode it prints each failure message before exiting.

The command validates the files present in the assembled tree, including generated scripts and configs. It does not
validate source repositories or files that were not included in the build.
