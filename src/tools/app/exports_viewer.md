# Exports Viewer

The exports viewer opens and displays script export declarations.

It is useful when checking available effects, conditions, dialogs, and parameter declarations used by game configs.

## Screens

- Navigator: open export sources.
- Exports view: browse parsed exports and filter declarations.

## Backend Commands

The Tauri backend exposes commands to:

- open and close export data;
- open and close `xr_effects` data;
- check whether effects data is loaded;
- get parsed exports and effects;
- parse `xr_effects` declarations.

Use this page as a reader and inspection tool. Source declarations still live in the engine repository.
