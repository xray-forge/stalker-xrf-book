# LTX CLI

LTX commands format and verify `.ltx` and `.ini` config files. Use them for standalone config projects or when you need
the lower-level tool behind the engine repository's `format ltx` and `verify ltx` commands.

## `format-ltx`

Formats one file or every LTX file under a folder.

```powershell
xrf-tool format-ltx --path ./gamedata/configs
xrf-tool format-ltx --path ./gamedata/configs --check
```

Options:

- `-p, --path <path>`: file or folder to format. Required.
- `-c, --check`: check formatting without rewriting project files.
- `-s, --silent`: disable logging.
- `-v, --verbose`: enable verbose logging.

`--check` is implemented for folders. Single-file mode formats the file.

## `verify-ltx`

Verifies an LTX project folder with scheme and strict-project options enabled.

```powershell
xrf-tool verify-ltx --path ./gamedata/configs
xrf-tool verify-ltx --path ./gamedata/configs --strict
```

Options:

- `-p, --path <path>`: configs root folder. Required.
- `--silent`: disable logging.
- `-v, --verbose`: enable verbose logging.
- `-s, --strict`: enable strict checking.

## Failure notes

`verify-ltx` expects a directory, not a single file. It fails when includes, inheritance, section fields, or scheme
validation produce errors.

Scheme definitions are documented in [Script config schemes](../../script_engine/configs_scheme.md).
