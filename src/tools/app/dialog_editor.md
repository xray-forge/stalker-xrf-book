# Dialog Editor

The dialog editor is a prototype for dialog graphs. It is not a production workflow for dialog data.

## Current routes

| Route                    | Purpose                                                   |
| ------------------------ | --------------------------------------------------------- |
| `/dialog_editor`         | Navigator with an **Open** entry.                         |
| Experimental graph route | Test graph page using React Flow dialog and phrase nodes. |

The current app source does not register a dialog-editor Tauri plugin and does not expose import, export, save, or
validation commands for real dialog XML.

## What is implemented

- A navigator page.
- A test graph with dialog and phrase node components.
- Client-side graph interactions through React Flow.

## What is not implemented

- Opening engine dialog XML.
- Saving dialog XML.
- Validating phrase links or script predicates/actions.
- Synchronizing dialog text with translation files.

## Production workflow

For real dialog changes, edit the engine sources directly:

- dialog XML under `src/engine/configs/gameplay`;
- dialog extern declarations under `src/engine/scripts/declarations/dialogs`;
- dialog text under `src/engine/translations`.

See [Dialog configs](../../script_engine/configs_dialogs.md) for the current production documentation.
