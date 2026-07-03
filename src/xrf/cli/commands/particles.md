# Particles

`particles` wraps the external XRF tools binary for packing and unpacking `particles.xr`.

```powershell
npm run cli -- particles <command>
```

## Commands

| Command            | Default input                      | Default output                     |
| ------------------ | ---------------------------------- | ---------------------------------- |
| `particles unpack` | `src/resources/particles.xr`       | `src/resources/particles_unpacked` |
| `particles pack`   | `src/resources/particles_unpacked` | `src/resources/particles.xr`       |

## Options

Both subcommands support:

- `-p, --path <path>`: source file or source directory.
- `-d, --dest <dest>`: output file or output directory.
- `-v, --verbose`: print verbose logs.
- `-f, --force`: remove an existing output before writing.

## Examples

```powershell
npm run cli -- particles unpack
npm run cli -- particles unpack --force
npm run cli -- particles pack
npm run cli -- particles pack --path src/resources/particles_unpacked --dest src/resources/particles.xr
```

## Workflow

Unpack first when you need to inspect or edit particle definitions as files. Pack after edits to rebuild `particles.xr`
for resources or packaging. Use `--force` when replacing a previous unpacked folder or packed output.

The command delegates to the bundled XRF tools binary. If you need lower-level particle conversion commands outside the
engine repository defaults, use the Tools CLI particle commands directly.

## Related verification

```powershell
npm run cli -- verify particles-packed
npm run cli -- verify particles-unpacked
```
