# Game SDK

The X-Ray Game SDK is the editor/toolchain used for assets such as levels, spawns, particles, models, and other
engine-native data.

XRF does not replace the SDK. The project adds source-controlled script/config/UI/translation workflows and helper tools
around game data, while SDK-style tools remain useful for native X-Ray asset authoring.

## When to Use the SDK

Use SDK tools when you need to work with data that is not represented well as text source:

- level editing;
- spawn and graph authoring;
- particle authoring;
- model or animation workflows;
- engine-native visual/editor data.

Use XRF source files and CLI tools when the change belongs to scripts, LTX/XML configs, UI XML, translations, or
repeatable validation.

## OpenXRay Reference

OpenXRay includes SDK-related work and documentation in its repository:

- [OpenXRay repository](https://github.com/OpenXRay/xray-16)
- [OpenXRay game editor wiki](https://github.com/OpenXRay/xray-16/wiki/%5BEN%5D-Game-Editor)

When SDK output is committed back to a project, keep generated binary output separate from hand-authored XRF sources.

## Boundary with XRF

Treat SDK output as source only when the project intentionally owns that binary or editor-authored asset. Do not edit
`target/` output by hand and do not treat packed game data as the source of truth.

For script/config changes, prefer XRF text sources and validation commands. For native asset changes, use the SDK or
format-specific XRF tools, then document how the generated asset should be reproduced.

## Common handoff pattern

1. Author or inspect the native asset in the SDK or a format-specific tool.
2. Export the asset into the project-owned resource location.
3. Rebuild or repack with XRF CLI commands.
4. Verify the resulting game data in game, especially for spawn, level, model, animation, and particle changes.
