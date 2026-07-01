# Archive CLI

Archive commands work with X-Ray `.db` archives.

## `unpack-archive`

Unpacks one archive project to a folder.

```powershell
xrf-tool unpack-archive --path gamedata.db0 --dest unpacked
```

Options:

- `-p, --path <path>`: path to a `.db` archive file. Required.
- `-d, --dest <dest>`: destination folder. Defaults to `unpacked`.
- `--parallel <count>`: number of parallel unpack workers. Defaults to `32`.
- `--dry`: read and summarize without writing files.
- `-s, --silent`: disable command logging.

The command prints archive count, file count, compressed size, and real size before unpacking unless silent mode is
enabled.
