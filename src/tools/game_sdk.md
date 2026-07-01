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
