# Archive CLI

Archive commands work with X-Ray `.db` database archives.

## `unpack-archive`

`unpack-archive` opens an archive project and exports the contained files to a folder.

```powershell
xrf-tool unpack-archive --path gamedata.db0 --dest unpacked
```

## Options

- `-p, --path <path>`: path to a `.db` archive file. Required.
- `-d, --dest <dest>`: destination folder. Defaults to `unpacked`.
- `--parallel <count>`: number of parallel unpack workers. Defaults to `32`.
- `--dry`: read and summarize the archive without writing files.
- `-s, --silent`: disable command logging.

Relative destination paths are resolved from the current working directory.

## Output

Without `--silent`, the command prints:

- archive count;
- file count;
- compressed size;
- real unpacked size;
- unpack duration when files are written.

## Examples

```powershell
xrf-tool unpack-archive --path .\db\configs.db0 --dest .\unpacked\configs
xrf-tool unpack-archive --path .\db\textures.db0 --dest .\unpacked\textures --parallel 8
xrf-tool unpack-archive --path .\db\sounds.db0 --dry
```
