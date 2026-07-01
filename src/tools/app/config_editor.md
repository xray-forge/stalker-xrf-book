# Config Editor

The config editor works with LTX config projects.

## Screens

- Explorer: browse config files.
- Verifier: run LTX verification for a selected path.
- Formatter: format LTX files or check whether formatting is needed.

## Backend Commands

The Tauri backend exposes commands to:

- verify a config path;
- format a config path;
- check whether a config path is already formatted.

Use this tool when you need a manual UI around the same kind of LTX checks used by the CLI.
