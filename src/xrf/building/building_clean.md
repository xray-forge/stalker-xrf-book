# Build Clean

Clean builds remove the previous `target/gamedata` output before the selected build targets run.

The option is implemented by the build command, not by individual config or script generators. When `--clean` is set,
the build process removes the whole `target/gamedata` directory before running the selected build filters.

Use a clean build when generated output may contain stale files, for example after deleting or renaming scripts,
configs, forms, translations, or resources:

```powershell
npm run cli -- build --clean
npm run cli -- build -c
```

The clean step removes `target/gamedata`. It does not change source files under `src/engine`, `src/resources`, or
external resource repositories.

## When to use it

Use a clean build after:

- deleting or renaming source files;
- changing build filters and wanting to remove output from previous filters;
- switching between resource sets or generated config layouts;
- preparing output before `compress`, `pack game`, or `pack mod`.

Skip it during tight iteration when you are only changing a file that the selected build step overwrites
deterministically.
