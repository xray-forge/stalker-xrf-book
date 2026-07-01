# LTX CLI

LTX commands format and verify `.ltx` and `.ini` config files.

## `format-ltx`

Formats a file or all LTX files under a folder.

```powershell
xrf-tool format-ltx --path ./gamedata/configs
xrf-tool format-ltx --path ./gamedata/configs --check
```

Options:

- `-p, --path <path>`: file or folder to format. Required.
- `-c, --check`: check formatting without rewriting project files.
- `-s, --silent`: disable logging.
- `-v, --verbose`: enable verbose logging.

## `verify-ltx`

Verifies an LTX project folder.

```powershell
xrf-tool verify-ltx --path ./gamedata/configs
xrf-tool verify-ltx --path ./gamedata/configs --strict
```

Options:

- `-p, --path <path>`: configs root folder. Required.
- `--silent`: disable logging.
- `-v, --verbose`: enable verbose logging.
- `-s, --strict`: enable strict checking.

Scheme definitions are documented in [Script config schemes](../../script_engine/configs_scheme.md).
