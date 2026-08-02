# Tools CLI

The Tools CLI is the Rust `xrf-cli` binary from `stalker-xrf-tools/bin/xrf-cli`. Use it for repeatable asset inspection,
conversion, packing, unpacking, formatting, and verification outside the engine repository wrapper.

```powershell
xrf-cli <command> --help
```

The engine CLI wraps some of these commands through `npm run cli -- ...`, but `xrf-cli` is the lower-level interface
used by both the engine scripts and the desktop tools.

## Command groups

| Group        | Commands                                                                                                               |
| ------------ | ---------------------------------------------------------------------------------------------------------------------- |
| Archive      | `unpack-archive`                                                                                                       |
| Gamedata     | `verify-gamedata`                                                                                                      |
| LTX          | `format-ltx`, `verify-ltx`                                                                                             |
| Models       | `info-ogf`, `info-omf`                                                                                                 |
| Particles    | `info-particles`, `unpack-particles`, `pack-particles`, `repack-particles`, `re-unpack-particles`, `verify-particles`  |
| Spawn        | `info-spawn`, `unpack-spawn`, `pack-spawn`, `repack-spawn`, `verify-spawn`                                             |
| Textures     | `info-dds`, `unpack-equipment-icons`, `pack-equipment-icons`, `unpack-texture-description`, `pack-texture-description` |
| Translations | `initialize-translation`, `build-translation`, `verify-translation`, `parse-translation`                               |

## Logging

Most commands support command-specific `--silent` or `--verbose` flags. The binary also initializes Rust logging from
`RUST_LOG` when the environment variable is present.

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
