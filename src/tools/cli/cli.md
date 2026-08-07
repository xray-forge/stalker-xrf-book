# Tools CLI

The Rust `xrf-cli` binary provides repeatable asset inspection, conversion, packing, unpacking, formatting, and
verification outside the engine repository wrapper.

```powershell
xrf-cli <command> --help
```

The engine CLI wraps selected operations through `npm run cli -- ...`. Use `xrf-cli` directly for scripts and
format-specific workflows.

## Command groups

| Group        | Commands                                                                                                               |
| ------------ | ---------------------------------------------------------------------------------------------------------------------- |
| Archive      | `unpack-archive`                                                                                                       |
| Externs      | `export-externs`                                                                                                       |
| Gamedata     | `verify-gamedata`                                                                                                      |
| LTX          | `format-ltx`, `verify-ltx`                                                                                             |
| Models       | `info-ogf`, `patch-ogf-motion-refs`, `info-omf`, `repack-omf`                                                          |
| Particles    | `info-particles`, `unpack-particles`, `pack-particles`, `repack-particles`, `re-unpack-particles`, `verify-particles`  |
| Spawn        | `info-spawn`, `unpack-spawn`, `pack-spawn`, `repack-spawn`, `verify-spawn`                                             |
| Textures     | `info-dds`, `unpack-equipment-icons`, `pack-equipment-icons`, `unpack-texture-description`, `pack-texture-description` |
| Translations | `initialize-translation`, `build-translation`, `verify-translation`, `parse-translation`                               |

## Logging

Some commands offer `--silent` or `--verbose`. Rust logging honors `RUST_LOG` when it is set.

## Read and write commands

Inspection commands such as `info-ogf`, `info-omf`, `info-dds`, `info-spawn`, and `info-particles` read input files and
print parsed metadata. Conversion commands such as archive unpacking, spawn packing, particle packing, texture packing,
and translation building write output paths.

Use explicit input and output paths when documenting or scripting a command. Some commands use named options such as
`--path` and `--dest`; `verify-gamedata` uses its assembled gamedata root as a positional operand. Explicit paths make
generated assets easier to reproduce.

## Usage pattern

Use the command-specific page when working with a file format. Each page lists the required input, output behavior, and
the commands that are safe to run as read-only inspection versus commands that write files.

When running from the engine repository, prefer the engine CLI wrapper if it already exposes the workflow. Use `xrf-cli`
directly when you need a lower-level command that the engine wrapper does not register. On Windows, use `xrf-cli.exe` or
the bundled path under `cli/bin/tools`.
