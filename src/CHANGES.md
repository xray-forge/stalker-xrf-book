# Changes

This page summarizes the main differences XRF introduces compared with a vanilla Call of Pripyat script and data setup.

## Development Workflow

- TypeScript source is compiled to Lua scripts with TypeScriptToLua.
- The repository includes a local CLI for building, linking, verification, packaging, engine switching, and asset tools.
- UI XML can be generated from JSX sources.
- LTX/XML configs can be generated from TypeScript sources.
- Translations can be generated from JSON sources and checked with CLI utilities.
- Jest tests and X-Ray/Lua fixtures cover runtime logic without launching the game.

## Build Pipeline

- Build targets are split into scripts, UI, configs, translations, and resources.
- Generated output is written to `target/gamedata`.
- Static resources can be copied from base, override, and locale-specific roots.
- Package commands can create mod or game output under `target`.
- Verification commands check project setup, gamedata, LTX, and particles data.

## Script Engine

- The original Lua script engine is rewritten in TypeScript.
- Runtime logic is organized into managers, schemes, binders, registries, utilities, and typed domain constants.
- The project uses typed declarations for X-Ray engine APIs through `xray16` and local typedefs.
- Events and callbacks are centralized so game systems can be tested and extended more predictably.

## Gameplay and Extensions

XRF aims to preserve the original plot and baseline behavior while making systems easier to test, optimize, and extend.
Larger gameplay changes should be isolated as extensions where possible.

Current extension-oriented work includes optional start-position behavior and other modular gameplay changes under
`src/engine/extensions`.

## Documentation and Tasks

Project task boards track ongoing work:

- [Script engine](https://github.com/orgs/xray-forge/projects/4)
- [Game engine](https://github.com/orgs/xray-forge/projects/6)
- [CLI and tools](https://github.com/orgs/xray-forge/projects/3)
- [Documentation](https://github.com/orgs/xray-forge/projects/5)
