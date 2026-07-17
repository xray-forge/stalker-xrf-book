# Translation CLI

Translation commands work with XRF JSON translation projects and generated gamedata string tables.

## Commands

| Command                  | Purpose                                                                                                                      |
| ------------------------ | ---------------------------------------------------------------------------------------------------------------------------- |
| `initialize-translation` | Ensure translation files have the expected language keys.                                                                    |
| `build-translation`      | Build translation JSON into gamedata output files.                                                                           |
| `verify-translation`     | Check translation completeness.                                                                                              |
| `parse-translation`      | Registered command for parsing XML translations; the current implementation accepts `--path` and returns without conversion. |

## Initialize

```powershell
xrf-cli initialize-translation --path ./translations
```

Options:

- `-p, --path <path>`: translation file or folder. Required.
- `-s, --silent`: disable logging.
- `-v, --verbose`: enable verbose logging.

## Build

```powershell
xrf-cli build-translation --path ./translations --output ./gamedata/configs/text --language ukr
```

Options:

- `-p, --path <path>`: translation file or folder. Required.
- `-o, --output <path>`: output folder. Required.
- `-l, --language <language>`: target language. Defaults to `all`.
- `-s, --silent`: disable logging.
- `-v, --verbose`: enable verbose logging.
- `--sort`: toggles sorting for dynamic translation files.

## Verify

```powershell
xrf-cli verify-translation --path ./translations --language ukr --strict
```

Options:

- `-p, --path <path>`: translation file or folder. Required.
- `-l, --language <language>`: target language. Defaults to `all`.
- `--strict`: exit with a non-zero status when translations are missing.
- `-s, --silent`: disable logging.
- `-v, --verbose`: enable verbose logging.

## Notes

The engine repository's `npm run cli -- translations ...` command provides a separate XML-to-JSON conversion workflow
implemented in TypeScript. Use that wrapper when converting original X-Ray XML string tables into XRF JSON sources.
