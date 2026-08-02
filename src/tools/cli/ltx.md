# LTX CLI

LTX commands format and verify `.ltx` and `.ini` config files. Use them for standalone config projects or when you need
the lower-level tool behind the engine repository's `format ltx` and `verify ltx` commands.

## `format-ltx`

Formats one file or every LTX file under a folder.

```powershell
xrf-cli format-ltx --path ./gamedata/configs
xrf-cli format-ltx --path ./gamedata/configs --check
```

Options:

- `-p, --path <path>`: file or folder to format. Required.
- `-c, --check`: check formatting without rewriting project files.
- `-s, --silent`: disable logging.
- `-v, --verbose`: enable verbose logging.

`--check` is implemented for folders. Single-file mode formats the file.

## `verify-ltx`

Verifies an LTX project folder, including its schemes and case-sensitive include paths.

```powershell
xrf-cli verify-ltx --path ./gamedata/configs
```

Options:

- `-p, --path <path>`: configs root folder. Required.
- `--silent`: disable logging.
- `-v, --verbose`: enable verbose logging.

## Failure notes

`verify-ltx` expects a directory, not a single file. It fails when includes, inheritance, section fields, or scheme
validation produce errors.

Only sections that declare `$schema` are checked against a scheme. Other sections, including array-style sections, are
valid without one. A scheme can use `$strict = true` when its own section shape is fully known.

Scheme definitions are documented in [Script config schemes](../../script_engine/configs_scheme.md).
