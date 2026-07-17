# Spawn CLI

Spawn commands inspect, verify, pack, unpack, and round-trip ALife `.spawn` files.

## Commands

| Command        | Purpose                                                         | Writes files |
| -------------- | --------------------------------------------------------------- | ------------ |
| `info-spawn`   | Print header, object, artefact spawn, patrol, and graph counts. | No           |
| `unpack-spawn` | Export a packed spawn file into a folder.                       | Yes          |
| `pack-spawn`   | Build a packed spawn file from an unpacked folder.              | Yes          |
| `repack-spawn` | Read a packed spawn file and write it to another packed file.   | Yes          |
| `verify-spawn` | Check whether a packed spawn file can be parsed.                | No           |

## Examples

```powershell
xrf-cli info-spawn --path ./all.spawn
xrf-cli unpack-spawn --path ./all.spawn --dest ./all_spawn --force
xrf-cli pack-spawn --path ./all_spawn --dest ./all.spawn --force
xrf-cli repack-spawn --path ./all.spawn --dest ./all.repacked.spawn
xrf-cli verify-spawn --path ./all.spawn
```

## Options

`pack-spawn`:

- `-p, --path <path>`: unpacked spawn folder. Required.
- `-d, --dest <dest>`: output `.spawn` file. Defaults to `unpacked`.
- `-f, --force`: remove an existing output file first.

`unpack-spawn`:

- `-p, --path <path>`: source `.spawn` file. Required.
- `-d, --dest <dest>`: output folder. Defaults to `unpacked`.
- `-f, --force`: remove an existing output folder first.
- `-s, --silent`: disable logging.

`info-spawn`, `verify-spawn`, and `repack-spawn` require `-p, --path <path>`. `repack-spawn` also requires
`-d, --dest <dest>`.

## Failure notes

Packing and unpacking reject existing destinations unless `--force` is supplied.
