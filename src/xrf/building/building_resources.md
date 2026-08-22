# Building Resources

The resources target copies static game assets into `target/gamedata`.

The base resource root is configured in `cli/config.json` as `resources.mod_assets_base_folder` and points to
`src/resources`. Additional override and locale roots can be configured in the same file.

## Build resources

```powershell
npm run cli -- build --include resources
npm run cli -- build -i resources
```

Use filters for focused resource copies:

```powershell
npm run cli -- build -i resources --filter textures
```

Filters are regular-expression strings matched against source file paths.

## Incremental builds

Directory resources are compared before copy. Non-clean builds skip unchanged files.

## Additional Assets

Clone configured resource repositories with:

```powershell
npm run cli -- clone --list
npm run cli -- clone extended
npm run cli -- clone locale-ukr
```

The relevant configuration keys are:

- `resources.mod_assets_override_folders` for general overrides;
- `resources.mod_assets_locales` for locale-specific assets.

Disable override roots for a build with `--no-asset-overrides`:

```powershell
npm run cli -- build --no-asset-overrides
```

## Resource Links

- [Base assets](https://gitlab.com/xray-forge/xrf-resources-base)
- [Extended assets](https://gitlab.com/xray-forge/xrf-resources-extended)
- [English locale assets](https://gitlab.com/xray-forge/xrf-resources-locale-en)
- [Ukrainian locale assets](https://gitlab.com/xray-forge/xrf-resources-locale-ukr)
- [Russian locale assets](https://gitlab.com/xray-forge/xrf-resources-locale-ru)
