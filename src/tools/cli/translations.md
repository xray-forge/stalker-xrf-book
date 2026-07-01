# Translation CLI

Translation commands work with XRF translation projects.

## Commands

- `build-translation`: build translation sources into gamedata output.
- `initialize-translation`: initialize translation files with expected keys.
- `parse-translation`: command stub for XML-to-JSON parsing. The current source registers the command but does not
  implement parsing behavior yet.
- `verify-translation`: check translation completeness.

## `build-translation`

```powershell
xrf-tool build-translation --path ./translations --output ./gamedata/configs/text --language ukr
```

Options:

- `-p, --path <path>`: translation file or folder. Required.
- `-o, --output <path>`: output folder. Required.
- `-l, --language <language>`: target language. Defaults to `all`.
- `-s, --silent`: disable logging.
- `-v, --verbose`: enable verbose logging.
- `--sort`: controls sorting for dynamic translation files.

## `initialize-translation`

```powershell
xrf-tool initialize-translation --path ./translations
```

Options:

- `-p, --path <path>`: translation file or folder. Required.
- `-s, --silent`: disable logging.
- `-v, --verbose`: enable verbose logging.

## `verify-translation`

```powershell
xrf-tool verify-translation --path ./translations --language ukr --strict
```

Options:

- `-p, --path <path>`: translation file or folder. Required.
- `-l, --language <language>`: target language. Defaults to `all`.
- `--strict`: exit with a non-zero code when translations are missing.
- `-s, --silent`: disable logging.
- `-v, --verbose`: enable verbose logging.
