# Particle CLI

Particle commands inspect, verify, pack, unpack, and round-trip `particles.xr` data.

## Commands

| Command               | Purpose                                                           | Writes files |
| --------------------- | ----------------------------------------------------------------- | ------------ |
| `info-particles`      | Print version, effect count, and group count.                     | No           |
| `unpack-particles`    | Export a packed `particles.xr` into a folder.                     | Yes          |
| `pack-particles`      | Build a packed `particles.xr` from an unpacked folder.            | Yes          |
| `repack-particles`    | Read a packed file and write it to another packed file.           | Yes          |
| `re-unpack-particles` | Read an unpacked folder and export it to another unpacked folder. | Yes          |
| `verify-particles`    | Check whether packed or unpacked particle data can be parsed.     | No           |

## Examples

```powershell
xrf-cli info-particles --path ./particles.xr
xrf-cli unpack-particles --path ./particles.xr --dest ./particles_unpacked --force
xrf-cli pack-particles --path ./particles_unpacked --dest ./particles.xr --force
xrf-cli repack-particles --path ./particles.xr --dest ./particles.repacked.xr
xrf-cli re-unpack-particles --path ./particles_unpacked --dest ./particles_unpacked_roundtrip
xrf-cli verify-particles --path ./particles.xr
xrf-cli verify-particles --path ./particles_unpacked --unpacked
```

## Shared options

Read commands require `-p, --path <path>`. Write commands that create output also use `-d, --dest <dest>`.
`unpack-particles` and `pack-particles` support `-f, --force` to remove an existing destination before writing.

## Failure notes

Packing fails if the output file already exists and `--force` is not supplied. Unpacking fails if the destination folder
already exists and `--force` is not supplied.
