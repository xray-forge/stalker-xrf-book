# Verify

`verify` validates project setup and generated data.

```powershell
npm run cli -- verify <command>
```

`npm run verify` runs `verify project`.

## Commands

| Command                     | Checks                              |
| --------------------------- |-------------------------------------|
| `verify project`            | Project setup and links.            |
| `verify gamedata`           | Assembled `target/gamedata`.        |
| `verify externs`            | Tracked extern manifest.            |
| `verify ltx`                | LTX structure and `$scheme` values. |
| `verify particles-packed`   | Packed `particles.xr`.              |
| `verify particles-unpacked` | Unpacked particle files.            |

## Options

- `verify gamedata -v, --verbose`: print verbose external-tool logs.
- `verify gamedata -s, --strict`: fully validate expensive asset payloads, including complete sound decoding.
- `verify ltx -v, --verbose`: print verbose external-tool logs.
- Particle verification commands support `-v, --verbose`.

## Examples

```powershell
npm run cli -- verify externs
npm run cli -- verify gamedata --verbose
```

Build gamedata before `verify gamedata`. `verify externs` does not write files. Regenerate a stale manifest with
`npm run cli -- build --include externs`.

## Failure notes

`verify project` reports setup problems without failing. Other checks fail on invalid data or tool errors.
