# Stalker XRF Book

This book documents the X-Ray Forge script engine, build pipeline, CLI tools, and supporting modding workflow for
S.T.A.L.K.E.R.: Call of Pripyat projects.

Use it when you need to:

- set up the XRF engine project locally;
- build scripts, configs, UI forms, translations, and resources into `target/gamedata`;
- use the local CLI for linking, verification, packaging, engine switching, and asset utilities;
- understand how the rewritten TypeScript script engine maps to X-Ray Lua scripts;
- debug game logic, UI, weather, logs, and runtime state.

The implementation source lives in `stalker-xrf-engine`. This book should describe behavior that is implemented there or
in the sibling XRF tool/resource repositories.

## Links

- [Xray16 type definitions](https://xray-forge.github.io/xray-16-types/modules.html)
- [XRF engine repository](https://github.com/xray-forge/stalker-xrf-engine)
- [XRF tools repository](https://github.com/xray-forge/stalker-xrf-tools)
- [XRF binaries repository](https://github.com/xray-forge/stalker-xrf-bin)
- [Book repository](https://github.com/xray-forge/stalker-xrf-book)

## Resource Repositories

- [Base assets](https://gitlab.com/xray-forge/stalker-xrf-resources-base)
- [Extended assets](https://gitlab.com/xray-forge/stalker-xrf-resources-extended)
- [English locale assets](https://gitlab.com/xray-forge/stalker-xrf-resources-locale-eng)
- [Ukrainian locale assets](https://gitlab.com/xray-forge/stalker-xrf-resources-locale-ukr)
- [Russian locale assets](https://gitlab.com/xray-forge/stalker-xrf-resources-locale-rus)

## References

- This site is built with [mdBook](https://github.com/rust-lang/mdBook).
- XRF targets [OpenXRay](https://github.com/OpenXRay/xray-16) as the main engine fork.
