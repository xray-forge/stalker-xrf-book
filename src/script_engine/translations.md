# Translations

Translations are string-table source files for UI labels, dialogs, tasks, item names, achievements, subtitles, and other
text shown by the game.

XRF keeps the main editable translation source in JSON files under `src/engine/translations`. The build converts those
sources into X-Ray string-table XML under `target/gamedata/configs/text`.

## Supported Languages

The supported locale keys are defined in `cli/config.json`:

| Key   | Language  |
| ----- | --------- |
| `eng` | English   |
| `fra` | French    |
| `ger` | German    |
| `ita` | Italian   |
| `pol` | Polish    |
| `rus` | Russian   |
| `spa` | Spanish   |
| `ukr` | Ukrainian |

The default build locale is also configured in `cli/config.json`. It can be overridden with the build command
`--language` option.

## Source Format

Each JSON file is a dictionary keyed by translation id. Each translation id contains one value per supported locale.
Values can be strings or string arrays:

```json
{
  "st_example_name": {
    "eng": "Example",
    "fra": "Example",
    "ger": "Example",
    "ita": "Example",
    "pol": "Example",
    "rus": "Example",
    "spa": "Example",
    "ukr": "Example"
  }
}
```

String arrays are used when XML text contains explicit line breaks. The XML import helper splits `\n` into arrays when
converting existing string-table XML into JSON.

## Build Behavior

The `translations` build target calls the bundled `xrf-cli` translation builder:

```powershell
npm run cli build -- --include translations
```

The source path is `src/engine/translations`. The target path is:

```text
target/gamedata/configs/text
```

Do not edit generated XML under `target/`. Fix the JSON source or the imported static XML source instead.

## Translation Utilities

The local CLI exposes helper commands under `translations`:

```powershell
npm run cli translations init <path>
npm run cli translations check
npm run cli translations check -- --language eng
npm run cli translations to_json <path> -- --language eng --output src/engine/translations
```

Use `init` to add missing locale keys to JSON translation files. Use `check` to list missing or invalid entries. Use
`to_json` when importing existing X-Ray XML string tables.

The XML importer accepts an optional `--encoding` value. If encoding is not passed, it tries the XML header first and
then falls back to `windows-1251` for `ukr` and `rus`, or `windows-1250` for the other supported locales.

## References From Game Data

Translation ids are referenced across multiple source types:

- UI form text nodes;
- dialog XML `text` nodes;
- task configs and task functors;
- item, weapon, outfit, and upgrade configs;
- script callbacks that return text ids.

When changing an id, search the repository before renaming it. Task fields and dialog conditions may be condlists or
script callbacks rather than plain string ids.

## Guidelines

- Keep ids stable when only the wording changes.
- Fill every supported locale key used by the file.
- Use arrays only for intentional multiline text.
- Run `npm run cli translations check` after translation edits.
- Run `npm run cli build -- --include translations` before packaging.
- Do not patch generated files under `target/gamedata/configs/text`.
