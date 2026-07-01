# Archive Editor

The archive editor opens X-Ray archive projects, lists archive contents, reads individual files, and unpacks archive
paths.

## Screens

- Navigator: open or browse an archive project.
- Editor: inspect loaded archive data and read files.
- Unpacker: unpack archive contents to a destination path.

## Backend Commands

The Tauri backend exposes commands to:

- open and close an archive project;
- check whether a project is loaded;
- get loaded project metadata;
- read a file from an archive;
- unpack archives from a path.

Use this tool when you need to inspect packed game resources without unpacking the whole game manually.
