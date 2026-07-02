# Tools CLI

The Tools CLI is the Rust `xrf-tool` binary from `stalker-xrf-tools/bin/xrf-cli`. Use it for repeatable asset
inspection, conversion, packing, unpacking, formatting, and verification outside the engine repository wrapper.

```powershell
xrf-tool <command> --help
```

The engine CLI wraps some of these commands through `npm run cli -- ...`, but `xrf-tool` is the lower-level interface
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

## Usage pattern

Use the command-specific page when working with a file format. Each page lists the required input, output behavior, and
the commands that are safe to run as read-only inspection versus commands that write files.
