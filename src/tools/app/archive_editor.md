# Archive Editor

The archive editor works with X-Ray `.db` archive projects. It can open an archive set, show project metadata, read
individual files as text, and unpack archives to a destination folder.

## Screens

| Screen      | Route                       | Purpose                                                           |
| ----------- | --------------------------- | ----------------------------------------------------------------- |
| Navigator   | `/archives_editor`          | Choose between opening an archive project and unpacking archives. |
| Open/editor | `/archives_editor/editor`   | Open an archive project and browse loaded archive data.           |
| Unpacker    | `/archives_editor/unpacker` | Select a packed archive folder and an output folder, then unpack. |

## Backend commands

The Tauri `archives-editor` plugin exposes:

- `open_archives_project`;
- `close_archives_project`;
- `has_archives_project`;
- `get_archives_project`;
- `read_archive_file`;
- `unpack_archives_path`.

`unpack_archives_path` opens the selected archive path as an `ArchiveProject` and unpacks it in parallel with `32`
workers.

## Workflow

1. Use **Open** to inspect archives without extracting everything.
2. Use file reading from the editor view when you need to inspect a text file inside the archive.
3. Use **Unpack** when you need the archive contents on disk.

Default project-aware paths point the unpacker at `target/game_link` for source archives and `target/unpacked_archives`
for output when an XRF project path is configured.

## CLI equivalent

Use `xrf-cli unpack-archive` for repeatable unpacking from scripts.
