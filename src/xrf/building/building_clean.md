# Build Clean

`build --clean` removes the previous `target/gamedata` tree, then runs the selected targets.

Use a clean build when generated output may contain stale files, for example after deleting or renaming scripts,
configs, forms, translations, or resources:

```powershell
npm run cli -- build --clean
npm run cli -- build -c
```

It never changes `src/engine`, `src/resources`, or configured external resource repositories.

## When to use it

Use a clean build after:

- deleting or renaming source files;
- changing build filters and wanting to remove output from previous filters;
- switching between resource sets or generated config layouts;
- preparing output before `compress`, `pack game`, or `pack mod`.

Skip it during tight iteration when the selected step deterministically overwrites the file you changed.
