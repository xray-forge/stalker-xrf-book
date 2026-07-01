# Texture CLI

Texture commands inspect DDS files and pack or unpack icon-related assets.

## Commands

- `info-dds`: print DDS size, metadata, mipmap, format, and compression information.
- `pack-equipment-icons`: pack separate equipment icons into a sprite.
- `unpack-equipment-icons`: unpack an equipment sprite into separate icons.
- `pack-texture-description`: pack icons based on an XML texture description.
- `unpack-texture-description`: unpack icons based on an XML texture description.

Use `xrf-tool <command> --help` for command-specific options. The engine repository wraps some of these operations
through its `icons` CLI command.
