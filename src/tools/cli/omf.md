# OMF CLI

OMF commands inspect and re-serialize X-Ray motion files.

## Commands

| Command      | Purpose                                                         | Writes files       |
| ------------ | --------------------------------------------------------------- | ------------------ |
| `info-omf`   | Print version, motions, bones, and animation parts.             | No                 |
| `repack-omf` | Read a motion file and write it back, or verify it round-trips. | Only with `--dest` |

## `info-omf`

```powershell
xrf-cli info-omf --path ./meshes/example.omf
```

Options:

- `-p, --path <path>`: path to an `.omf` file. Required.

### Output

The command reads the motion file and prints:

- OMF version;
- motion count and motion names;
- total bone count;
- animation part names;
- bones assigned to each animation part.

### When to use it

Use `info-omf` when checking whether a motion file is readable, whether expected motions are present, or how motion
parts map to skeleton bones.

The command is read-only and prints the parsed structure; it does not merge, split, or repair motion files. If an
expected animation is absent, check the source OMF first and then inspect the model or config that references the motion
name.

## `repack-omf`

```powershell
xrf-cli repack-omf --path ./meshes/example.omf --dest ./meshes/example.repacked.omf
xrf-cli repack-omf --path ./meshes/example.omf --verify
xrf-cli repack-omf --path ./meshes
```

Options:

- `-p, --path <path>`: path to an `.omf` file, or to a directory containing `.omf` files. Required.
- `-d, --dest <dest>`: path to the resulting `.omf` file. Required when writing a single file. Rejected when `--path` is
  a directory.
- `--verify`: compare the re-serialized bytes against the source instead of writing output.

The command has three modes:

- **Write.** A file path plus `--dest` reads the source and writes a re-serialized copy.
- **Verify one file.** A file path plus `--verify` re-serializes in memory and compares against the source bytes.
  Nothing is written.
- **Verify a directory.** A directory path walks it recursively, verifies every `.omf` beneath it, and prints a summary.
  Nothing is written, and `--dest` is rejected.

Unmodified files round-trip byte for byte. The writer preserves the original chunk order and reproduces the nested
motion chunk ids, so a repacked file that differs from its source indicates a real change, not serialization noise.

### When to use it

Use directory verification as a regression check after changing OMF parsing or serialization, and before relying on
generated motion banks. Because it fails on any mismatch, it works as a build or pre-commit gate.

Use single-file write mode when you need a normalized copy of a motion file.

## Shared options

Both commands accept `-s, --silent` to disable logging and `-v, --verbose` to enable verbose logging.

Under `repack-omf`, `--verbose` additionally reports every file that verified as byte identical. Without it, verifying a
directory prints only mismatches, read failures, and a closing summary, and verifying a single file that passes prints
nothing at all. Scripts can therefore treat single-file verification as silent on success and rely on the exit status.

## Failure notes

`repack-omf` exits with a non-zero status when any file mismatches or cannot be processed. In directory mode it also
reports the count of mismatched and errored files. A successful run exits zero.

A file that cannot be read fails before anything is written. The most common cause is a truncated source: a chunk header
declares more bytes than the file actually contains. Such files are damaged at the source and need to be re-extracted,
usually from the original packed archive with `unpack-archive` rather than from an unpacked dump.

Writing rejects data it cannot represent faithfully rather than emitting a corrupt file. Motion marks exist only in
version 4, so writing a version 3 file that carries marks fails, as does writing a file whose motion count and motion
definition count disagree.
