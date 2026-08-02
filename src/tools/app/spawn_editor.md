# Spawn Editor

The spawn editor reads and inspects ALife spawn data. It can open packed files, import and export unpacked folders, and
save a packed file.

## Screens

| Screen    | Route                  | Purpose                                          |
| --------- | ---------------------- | ------------------------------------------------ |
| Navigator | `/spawn_editor`        | Choose Open, Unpack, or Pack.                    |
| Editor    | `/spawn_editor/editor` | Open and inspect the current spawn file.         |
| Unpack    | `/spawn_editor/unpack` | Unpack a spawn file into a folder.               |
| Pack      | `/spawn_editor/pack`   | Pack an unpacked spawn folder into a spawn file. |

## Data views

The backend exposes accessors for:

- header data;
- graph data;
- ALife spawn objects;
- artefact spawn points;
- patrols;
- full loaded spawn file data.

## Backend commands

The Tauri `spawns-editor` plugin exposes:

- `open_spawn_file`;
- `import_spawn_file`;
- `export_spawn_file`;
- `save_spawn_file`;
- `close_spawn_file`;
- `get_spawn_file`;
- `get_spawn_file_header`;
- `get_spawn_file_graphs`;
- `get_spawn_file_alife_spawns`;
- `get_spawn_file_artefact_spawns`;
- `get_spawn_file_patrols`;
- `has_spawn_file`.

## Workflow

1. Use **Open** for a packed `all.spawn` file.
2. Inspect header, graph, ALife, artefact, and patrol chunks in the editor.
3. Use **Unpack** to export a packed spawn file into editable structured data.
4. Use **Pack** to rebuild a packed spawn file from an unpacked folder.

When an XRF project path is configured, default paths point at `target/gamedata/spawns/all.spawn`,
`target/spawns/unpacked`, and `target/spawns/repacked/repacked.spawn`.

## Safety

Spawn files are binary game data. Keep a backup before saving or replacing a spawn file.
