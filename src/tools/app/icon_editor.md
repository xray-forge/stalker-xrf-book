# Icon Editor

The icon editor works with equipment icon sprites and XML texture descriptions.

## Screens

- Equipment sprite viewer/editor.
- Equipment icon pack page.
- Equipment icon unpack page.
- Texture-description open page.
- Texture-description pack page.
- Texture-description unpack page.

## Backend Commands

The Tauri backend exposes commands to:

- open, close, and reopen an equipment sprite;
- get the currently loaded equipment sprite;
- pack equipment icons.

Description pack/unpack screens are routed in the UI. Check the current source before relying on a specific backend
operation for texture-description workflows.
