# XRF Tools

`stalker-xrf-tools` is the companion tools workspace for XRF development. It contains reusable Rust crates, a CLI, and a
Tauri desktop application.

Use it for tasks that are awkward to do by hand:

- reading and unpacking X-Ray archives;
- verifying and formatting LTX configs;
- converting and checking translations;
- inspecting script exports;
- packing and unpacking equipment icons and texture descriptions;
- inspecting or converting spawn, particles, OGF, and OMF data.

## Repository Layout

- `crates/`: reusable Rust crates for X-Ray formats and project validation.
- `bin/xrf-cli`: command-line tool.
- `bin/xrf-app`: Tauri backend for the desktop application.
- `bin/xrf-ui`: React frontend for the desktop application.

The engine repository uses a bundled tools binary from `cli/bin` for some build and asset operations.

## Interfaces

Use the desktop app for manual inspection and editing workflows. Use the CLI for repeatable scripts, CI checks, and
engine build integration.
