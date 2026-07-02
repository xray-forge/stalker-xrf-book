# Spawn

`spawn` contains ALife spawn file utilities. The engine CLI currently exposes the unpack workflow.

```powershell
npm run cli -- spawn unpack
```

## Defaults

| Field       | Default                          |
| ----------- | -------------------------------- |
| Source      | `src/resources/spawns/all.spawn` |
| Destination | `target/all_spawn`               |

The command delegates to `cli/bin/tools/xrf-cli unpack-spawn`.

## Options

- `-p, --path <path>`: source spawn file path.
- `-d, --dest <dest>`: output directory.
- `-v, --verbose`: print verbose logs.
- `-f, --force`: remove an existing unpacked destination before writing.

## Examples

```powershell
npm run cli -- spawn unpack
npm run cli -- spawn unpack --force
npm run cli -- spawn unpack --path src/resources/spawns/all.spawn --dest target/all_spawn
```

## Failure notes

The source spawn file must exist. Use the Tools CLI spawn commands when you need lower-level spawn info, pack, repack,
or verification operations.
