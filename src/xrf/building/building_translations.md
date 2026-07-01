# Building Translations

The translations build target writes text files to `target/gamedata/configs/text`.

Translation sources live in `src/engine/translations`. The build step delegates to the bundled XRF tools binary and
passes the source path, output path, and verbosity mode.

## Build Only Translations

```powershell
npm run cli -- build --include translations
npm run cli -- build -i translations
```

## Selecting a Language

The default locale is configured in `cli/config.json` as `locale`. The current default is `ukr`.

Override it for a build with `--language`:

```powershell
npm run cli -- build --include translations --language eng
```

Supported locale keys are listed in `cli/config.json` under `available_locales`.

## Locale Resource Packs

Resource packs for voice and localized assets are configured under `resources.mod_assets_locales` in `cli/config.json`.
When asset overrides are enabled, the resources build includes override folders plus the locale folders for the selected
language.

## JSON and XML Sources

XRF uses JSON translation sources for generated multilingual output and can also copy static XML translation files where
they are part of the source tree. Use the translation CLI commands when converting existing game XML into JSON sources.
