# Archive CLI

Archive commands work with X-Ray `.db` database archives.

## `pack-archive`

`pack-archive` builds database archives from a folder, replacing the `xrCompress` tool of the original SDK.

```powershell
xrf-cli pack-archive --path target\gamedata --dest target\db --name gamedata
```

A set that fits in one volume is written as `<name>.db`; a larger one splits into `<name>.db0`, `<name>.db1` and so on.
The engine mounts any file whose extension starts with `db` or `xdb`, so the index is a convenience rather than a
requirement. Files the engine expects compressed are compressed and the rest are stored, matching what the engine loads.
Identical files are stored once and referenced twice.

Give the archive a `[header]`. Without one the engine assumes a `.db` is an encrypted Shadow of Chernobyl archive and
decrypts it into nonsense; `--xdb` is the other way to say an archive is not that.

### Options

- `-p, --path <path>`: folder to pack, normally a `gamedata` root. Required.
- `-d, --dest <dest>`: destination folder for the volumes. Defaults to `packed`.
- `-n, --name <name>`: base name of the volumes. Defaults to `gamedata`.
- `--ltx <path>`: configuration file selecting what to include. Options given on the command line win over it.
- `--store`: store every file instead of compressing.
- `--max-size <megabytes>`: maximum volume size. Defaults to and is capped at 1900.
- `--xdb`: write volumes with the `xdb` extension.
- `--no-skip-list`: keep editor and source leftovers the game build normally drops.
- `-s, --silent`: disable command logging.

### Configuration file

Without `--ltx` the whole folder is packed. The file uses the same dialect `xrCompress` accepted:

```ini
[options]
exclude_exts = *.txt,*.json

[include_folders]
configs = true
scripts = true

[include_files]
gamemtl.xr

[header]
auto_load = true
entry_point = $fs_root$\gamedata\
```

`[include_folders]` and `[exclude_folders]` map a path to whether it applies recursively; `.\` names the packed root.
`[include_files]` lists names one per line. `[header]` is written into the archive verbatim, and its `entry_point` is
where the engine mounts the contents, so an archive of a `gamedata` tree needs it.

## `unpack-archive`

`unpack-archive` opens an archive project and exports the contained files to a folder.

```powershell
xrf-cli unpack-archive --path gamedata.db0 --dest unpacked
```

### Options

- `-p, --path <path>`: path to a `.db` archive file. Required.
- `-d, --dest <dest>`: destination folder. Defaults to `unpacked`.
- `--parallel <count>`: number of parallel unpack workers. Defaults to `32`.
- `--dry`: read and summarize the archive without writing files.
- `-s, --silent`: disable command logging.

Relative destination paths are resolved from the current working directory.

### Output

Without `--silent`, the command prints:

- archive count;
- file count;
- compressed size;
- real unpacked size;
- unpack duration when files are written.

With `--dry`, the command still reads the archive metadata but does not write the extracted files. Use it to confirm
that a database can be opened before spending time on a full unpack.

### Examples

```powershell
xrf-cli unpack-archive --path .\db\configs.db0 --dest .\unpacked\configs
xrf-cli unpack-archive --path .\db\textures.db0 --dest .\unpacked\textures --parallel 8
xrf-cli unpack-archive --path .\db\sounds.db0 --dry
```

### Failure notes

The source path must point to a readable X-Ray database archive. If the destination already contains files, choose a new
folder or clean it before running the command.
