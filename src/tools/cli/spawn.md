# Spawn CLI

Spawn commands work with ALife `.spawn` files.

## Commands

- `pack-spawn`: pack an unpacked spawn folder into a single `.spawn` file.
- `repack-spawn`: read a `.spawn` file and write it to another file.
- `unpack-spawn`: unpack a `.spawn` file into separate files.
- `verify-spawn`: verify that a spawn file can be read.
- `info-spawn`: print header, object, artefact spawn, patrol, and graph counts.

## `pack-spawn`

```powershell
xrf-tool pack-spawn --path ./unpacked --dest ./all.spawn --force
```

Options:

- `-p, --path <path>`: unpacked spawn folder. Required.
- `-d, --dest <dest>`: output `.spawn` file. Defaults to `unpacked`.
- `-f, --force`: remove an existing output file first.

## `repack-spawn`

```powershell
xrf-tool repack-spawn --path ./all.spawn --dest ./all.repacked.spawn
```

Options:

- `-p, --path <path>`: source `.spawn` file. Required.
- `-d, --dest <dest>`: output `.spawn` file. Required.

## `unpack-spawn`

```powershell
xrf-tool unpack-spawn --path ./all.spawn --dest ./unpacked --force
```

Options:

- `-p, --path <path>`: source `.spawn` file. Required.
- `-d, --dest <dest>`: output folder. Defaults to `unpacked`.
- `-f, --force`: remove an existing output folder first.
- `-s, --silent`: disable logging.

## `verify-spawn` and `info-spawn`

Both commands require `-p, --path <path>`.
