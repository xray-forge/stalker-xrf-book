# XRF Engine

XRF is a TypeScript rewrite of the S.T.A.L.K.E.R.: Call of Pripyat script engine. The source is compiled to Lua scripts
with TypeScriptToLua and packaged with generated configs, UI XML, translations, and static game resources.

The engine repository also includes a Node-based CLI for common modding tasks:

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

## Development Model

Most work starts in TypeScript or source data files and then goes through the CLI build pipeline. The generated files
are runtime artifacts for X-Ray; the source of truth stays in the repository.
