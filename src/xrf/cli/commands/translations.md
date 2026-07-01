# Translations

Provides translation utilities.

```powershell
npm run cli -- translations <command>
```

## Commands

- `translations init <path>`: initialize a JSON translation file with all language keys.
- `translations to_json <path>`: convert XML translation files or folders to JSON.
- `translations check`: list missing or wrong translations.

## Options

`to_json` supports:

- `-l, --language <locale>`: language key for converted translations.
- `-c, --clean`: clean output before writing.
- `-o, --output <path>`: output file or directory.
- `-e, --encoding <encoding>`: source XML encoding.
- `-v, --verbose`: print verbose logs.

`check` supports:

- `-l, --language <locale>`: check one language.
- `-s, --strict`: fail on missing entries.
- `-v, --verbose`: print verbose logs.

## Examples

```powershell
npm run cli -- translations to_json .\configs\text\eng\ --language eng --output .\src\engine\translations
npm run cli -- translations check
npm run cli -- translations check --language ukr --strict
```
