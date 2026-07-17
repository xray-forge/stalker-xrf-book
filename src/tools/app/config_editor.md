# Config Editor

The config editor works with LTX config projects. The current production-ready workflows are verification and
formatting.

## Screens

| Screen    | Route                       | Purpose                                                                                            |
| --------- | --------------------------- | -------------------------------------------------------------------------------------------------- |
| Navigator | `/configs_editor`           | Choose Explorer, Verifier, or Formatter.                                                           |
| Explorer  | `/configs_editor/explorer`  | Route and form shell for opening config folders. The current source does not wire the open action. |
| Verifier  | `/configs_editor/verifier`  | Verify an LTX project folder and display validation errors.                                        |
| Formatter | `/configs_editor/formatter` | Check formatting or format all LTX files under a folder.                                           |

## Backend commands

The Tauri `configs-editor` plugin exposes:

- `verify_configs_path`;
- `check_format_configs_path`;
- `format_configs_path`.

Verification opens the folder with scheme checking enabled and strict checking disabled. Formatting uses the shared
`xray-ltx` formatter.

## Workflow

1. Select the configs folder. When an XRF project path is configured, the app can prefill `src/engine/configs`.
2. Run **Verifier** after config edits to inspect include, inheritance, section, and scheme errors.
3. Run **Formatter** in check mode first when reviewing changes.
4. Disable check mode only when you want the formatter to rewrite files.

## CLI equivalent

Use `xrf-cli verify-ltx` and `xrf-cli format-ltx` for repeatable checks. In the engine repository, use
`npm run cli -- verify ltx` and `npm run cli -- format ltx`.
