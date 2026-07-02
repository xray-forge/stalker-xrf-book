# Engine Debug (C++)

Use engine debugging when the problem crosses from TypeScript/Lua into xray runtime behavior: scheduler timing,
rendering, physics, UI initialization, luabind bindings, or console command implementation.

For script-only behavior, start with logs and the XRF debug panel first. They are faster and do not require rebuilding
the engine.

## Set Up the Engine Project

OpenXRay has build guides for both supported development paths:

- [OpenXRay Windows build guide](https://github.com/OpenXRay/xray-16/wiki/%5BEN%5D-How-to-build-and-setup-on-Windows)
- [OpenXRay Linux build guide](https://github.com/OpenXRay/xray-16/wiki/%5BEN%5D-How-to-build-and-setup-on-Linux)

For local XRF development on Windows, the usual flow is:

1. Build or select a mixed/debug-capable engine.
2. Link XRF output and logs to the game folder with the XRF CLI link command.
3. Start the engine from Visual Studio when you need C++ breakpoints.
4. Start from the XRF CLI when you only need game logs and Lua output.

## Debugging Lua from Visual Studio

Lua debugging is possible through the Visual Studio Lua debugger extension, but it is limited by the engine and by the
compiled Lua output.

1. Install the [LuaDkmDebugger](https://github.com/WheretIB/LuaDkmDebugger) Visual Studio extension.
2. Use an engine configuration that loads Lua debug symbols.
3. Run the game from Visual Studio.
4. Set breakpoints in the generated Lua files, not in TypeScript source.

XRF TypeScript is compiled to Lua by TypeScriptToLua. Visual Studio will see the generated Lua code that the engine
executes.

## What to Debug in C++

Use the engine source when you need to verify:

- console command behavior such as `rs_stats`, `rs_fps`, `ai_stats`, `set_weather`, or `run_string`;
- luabind signatures exposed through `level`, `game`, `game_object`, UI classes, planners, and packets;
- UI XML initialization behavior in `CScriptXmlInit` and CUI controls;
- engine-only debug overlays, stats, scheduler, physics, and renderer behavior.

Use `C:\Projects\stalker\xray-16-types` first for the TypeScript-visible API shape, then confirm ambiguous runtime
semantics in `C:\Projects\stalker\xray-16`.

## Limitations

- TypeScript breakpoints are not available in the engine. Debug generated Lua or C++ instead.
- LuaDkmDebugger support is old and does not reliably inspect every luabind class or userdata value.
- Some debug console commands are compiled only into mixed/debug engine builds.
- Optimized script builds can strip Lua logger calls when built with `--no-lua-logs`.
