# Spawn Editor

The spawn editor opens and inspects ALife spawn files.

## Screens

- Navigator: open a spawn file.
- Editor: inspect loaded spawn chunks.
- Pack: pack spawn data.
- Unpack: unpack a spawn file.

## Data Views

The editor has backend accessors for:

- header data;
- graph data;
- ALife spawn objects;
- artefact spawn points;
- patrols.

## Backend Commands

The Tauri backend exposes commands to:

- open and close a spawn file;
- check whether a spawn file is loaded;
- get spawn file data and individual chunk groups;
- import and export spawn data;
- save a spawn file.

Use this tool for inspection before editing binary spawn data. Keep backups when saving converted spawn files.
