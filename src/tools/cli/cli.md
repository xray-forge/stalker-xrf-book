# Tools CLI

The tools CLI is the Rust command-line application from `stalker-xrf-tools/bin/xrf-cli`. It is intended for repeatable
asset conversion, verification, and inspection tasks.

The binary name in source is `xrf-tool`.

```powershell
xrf-tool <command> --help
```

## Command Groups

- Archive: `unpack-archive`
- Gamedata: `verify-gamedata`
- LTX: `format-ltx`, `verify-ltx`
- OGF: `info-ogf`
- OMF: `info-omf`
- Particles: `info-particles`, `pack-particles`, `repack-particles`, `reunpack-particles`, `unpack-particles`,
  `verify-particles`
- Spawn: `info-spawn`, `pack-spawn`, `repack-spawn`, `unpack-spawn`, `verify-spawn`
- Textures: `info-dds`, `pack-equipment-icons`, `pack-texture-description`, `unpack-equipment-icons`,
  `unpack-texture-description`
- Translations: `build-translation`, `initialize-translation`, `parse-translation`, `verify-translation`

Use the linked pages for the commands currently documented in this book.
