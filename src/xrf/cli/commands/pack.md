# Pack

`pack` creates distributable mod or game folders from the project.

```powershell
npm run cli -- pack <type>
```

`<type>` must be `mod` or `game`.

## Output

| Type   | Output folder         | Contents                                                                        |
| ------ | --------------------- | ------------------------------------------------------------------------------- |
| `mod`  | `target/mod_package`  | `gamedata`, and optionally bundled engine binaries.                             |
| `game` | `target/game_package` | engine `bin`, root assets, `gamedata`, and optionally compressed `db` archives. |

## Options

- `--nb, --no-build`: package already built assets without running `build`.
- `--nc, --no-compress`: for game packages, skip archive compression and copy all `gamedata`.
- `--na, --no-asset-overrides`: pass through to build and skip override/locale resource roots.
- `-e, --engine <type>`: use a bundled engine from `cli/bin/engines`.
- `--se, --skip-engine`: do not include engine binaries. This is allowed for `mod` packages and rejected for `game`.
- `-o, --optimize`: build scripts without Lua logs.
- `-v, --verbose`: print verbose logs.
- `-c, --clean`: remove the package output folder first.

## Examples

```powershell
npm run cli -- pack mod --clean
npm run cli -- pack mod --skip-engine --no-build
npm run cli -- pack game --clean --optimize
npm run cli -- pack game --engine release
npm run pack:mod
npm run pack:game
```

## Failure notes

Game packages require a valid bundled engine. Compressed game packages also require successful build and compression
steps, because `target/db` is copied into the package.
