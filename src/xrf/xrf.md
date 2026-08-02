# XRF Engine

XRF is a TypeScript implementation of the Call of Pripyat script layer. The build compiles it to Lua and assembles
configs, UI XML, translations, and static resources into `target/gamedata`.

The engine repository also includes a local Node CLI for common modding tasks:

- building `target/gamedata`;
- linking the project to a local game installation;
- switching bundled OpenXRay engine binaries;
- verifying project data;
- packing mod or game distributions;
- formatting and checking generated data sources.

## Source Layout

The main engine source is under `src/engine`:

- `scripts` contains TypeScript entry points that become Lua scripts.
- `core` contains runtime systems, schemes, managers, objects, and utilities.
- `configs` contains static and generated LTX/XML config sources.
- `forms` contains JSX/XML UI form sources.
- `translations` contains JSON and XML translation sources.
- `extensions` contains optional gameplay modules.

Build output and generated artifacts go under `target/`.

## Development model

Edit source files, then build. `target/` is generated output; do not edit it by hand.
