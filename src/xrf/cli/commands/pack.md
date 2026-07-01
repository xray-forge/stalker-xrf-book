# Pack

Creates mod or game packages from the XRF project.

```powershell
npm run cli -- pack <type>
```

`<type>` is the package type handled by the pack command, commonly `mod` or `game`.

## Options

- `--nb, --no-build`: do not run the build step before packing.
- `--nc, --no-compress`: do not run compression.
- `--na, --no-asset-overrides`: skip configured override and locale asset roots.
- `-e, --engine <type>`: use a specific bundled engine.
- `--se, --skip-engine`: do not include an engine in the package.
- `-o, --optimize`: use build optimizations.
- `-v, --verbose`: print verbose logs.
- `-c, --clean`: clean the destination before packing.

## Examples

```powershell
npm run cli -- pack game --clean --optimize
npm run cli -- pack game -c -o
npm run pack:game
npm run pack:mod
```

Game packages need full configured resources, including extended assets and the selected locale resource pack.
