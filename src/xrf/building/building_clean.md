# Build Clean

Clean builds remove the previous `target/gamedata` output before the selected build targets run.

Use a clean build when generated output may contain stale files, for example after deleting or renaming scripts,
configs, forms, translations, or resources:

```powershell
npm run cli -- build --clean
npm run cli -- build -c
```

The clean step removes `target/gamedata`. It does not change source files under `src/engine`, `src/resources`, or
external resource repositories.
