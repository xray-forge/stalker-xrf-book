# CLI Configuration

The engine CLI reads its defaults from `cli/config.json`.

This file controls:

- default locale and supported locale keys;
- base, override, and locale resource roots;
- source paths for configs, scripts, translations, extensions, and UI forms;
- `target` output path;
- compression and helper binary paths;
- default package engine and root package assets;
- game executable and fallback game path.

## Locale

`locale` is the default language used by builds. `available_locales` defines the accepted locale keys.

Override the locale for a build with:

```powershell
npm run cli -- build --language eng
```

Locale-specific resource roots are configured in `resources.mod_assets_locales`.

## Resources

`resources.mod_assets_base_folder` points to the base resources folder. In the engine repo this is `src/resources`.

`resources.mod_assets_override_folders` lists additional asset roots. `resources.mod_assets_locales` maps locale keys to
locale resource roots. These roots are included by the resources build unless `--no-asset-overrides` is passed.

## Build Paths

The `build` section maps source folders:

- `configs` -> `src/engine/configs`
- `scripts` -> `src/engine/scripts`
- `translations` -> `src/engine/translations`
- `extensions` -> `src/engine/extensions`
- `ui` -> `src/engine/forms`

Generated output goes under `target/gamedata`.

## Non-Steam Game Path

The `targets` section contains the Steam app id, fallback game path, and game executable name. Update
`stalker_game_fallback_path` if the CLI cannot locate a non-Steam installation automatically.
