# Verify

Runs project and data validation commands.

```powershell
npm run cli -- verify <command>
```

## Commands

- `verify project`: verify project setup and links.
- `verify gamedata`: verify gamedata integrity.
- `verify ltx`: verify LTX project integrity and schemes.
- `verify particles-packed`: verify packed particles.
- `verify particles-unpacked`: verify unpacked particles.

## Options

`gamedata`, `ltx`, and particle checks support `-v, --verbose`.

`verify ltx` also supports `-s, --strict`.

## Examples

```powershell
npm run verify
npm run cli -- verify ltx
npm run cli -- verify ltx --strict
```
