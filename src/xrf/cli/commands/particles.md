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

## Related verification

```powershell
npm run cli -- verify particles-packed
npm run cli -- verify particles-unpacked
```
