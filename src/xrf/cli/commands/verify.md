# Verify

`verify` runs project and gamedata validation commands.

```powershell
npm run cli -- verify <command>
```

The package script `npm run verify` is shorthand for `npm run cli -- verify project`.

## Commands

| Command                     | Checks                                                                                                   |
| --------------------------- | -------------------------------------------------------------------------------------------------------- |
| `verify project`            | CLI config, game path, game executable, engine link, gamedata link, logs link, and configured resources. |
| `verify gamedata`           | Asset references across configured resource roots and config sources.                                    |
| `verify ltx`                | LTX includes, inheritance, section shape, and `$scheme` validation.                                      |
| `verify particles-packed`   | Packed `src/resources/particles.xr`.                                                                     |
| `verify particles-unpacked` | Unpacked particles under `src/resources/particles_unpacked`.                                             |

## Options

- `verify gamedata -v, --verbose`: print verbose external-tool logs.
- `verify ltx -s, --strict`: run strict LTX validation.
- `verify ltx -v, --verbose`: print verbose external-tool logs.
- Particle verification commands support `-v, --verbose`.

## Examples

```powershell
npm run verify
npm run cli -- verify project
npm run cli -- verify ltx
npm run cli -- verify ltx --strict
npm run cli -- verify gamedata --verbose
npm run cli -- verify particles-packed
```

## Failure notes

`verify project` reports setup problems but catches its own top-level error. `verify ltx` and particle verification
delegate to the tools binary and fail the process on invalid data.
