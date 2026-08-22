# X-Ray Forge

X-Ray Forge (XRF) is a TypeScript rewrite of the S.T.A.L.K.E.R.: Call of Pripyat script layer. It compiles gameplay code
to Lua and builds the configs, UI forms, translations, and resources used by X-Ray.

This book covers the full development workflow: project setup, builds, CLI and desktop tools, runtime architecture,
gameplay schemes, and debugging.

## Start here

- New to XRF? Follow [Installation](./INSTALLATION.md), then read [Building](./xrf/building/building.md).
- Working on gameplay? Start with the [Script engine](./script_engine/script_engine.md) and
  [Schemes](./script_engine/schemes/schemes.md).
- Looking for a command? See the [XRF CLI](./xrf/cli/cli.md) or [Tools CLI](./tools/cli/cli.md).
- Investigating runtime behavior? Read [Game engine](./game_engine/game_engine.md) and
  [Debugging](./debugging/debugging.md).

Generated files are written under `target/`. Edit sources in the engine or tools repository and rebuild; do not edit
generated gamedata by hand.

## Project repositories

- [XRF engine](https://github.com/xray-forge/xrf-engine) — script runtime and project CLI.
- [XRF tools](https://github.com/xray-forge/xrf-tools) — format libraries, CLI, and desktop application.
- [XRF binaries](https://github.com/xray-forge/xrf-bin) — packaged engine and tool binaries.
- [XRF X-Ray 16 SDK](https://xray-forge.github.io/xrf-xray16-sdk/modules.html) — TypeScript-facing engine API.
- [Book source](https://github.com/xray-forge/xrf-book) — this documentation.

XRF uses [OpenXRay](https://github.com/OpenXRay/xray-16) as its main engine reference. Game assets are split across
[base](https://gitlab.com/xray-forge/xrf-resources-base),
[extended](https://gitlab.com/xray-forge/xrf-resources-extended), and locale repositories for
[English](https://gitlab.com/xray-forge/xrf-resources-locale-en),
[Ukrainian](https://gitlab.com/xray-forge/xrf-resources-locale-ukr), and
[Russian](https://gitlab.com/xray-forge/xrf-resources-locale-ru).
