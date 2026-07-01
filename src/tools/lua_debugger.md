# Lua Debugger

Lua debugging for X-Ray scripts is limited by the engine runtime, luabind objects, and the fact that XRF source starts
as TypeScript before being emitted as Lua.

For most day-to-day script work, start with:

- focused Jest tests around TypeScript source;
- XRF logs and game logs;
- generated Lua inspection under `target/gamedata`;
- engine-side debugging when the issue crosses into C++ or luabind behavior.

## Breakpoints

Breakpoints in original TypeScript source are not equivalent to breakpoints in emitted Lua. If you attach a Lua
debugger, set breakpoints against generated Lua paths and verify that the generated script names match what the engine
loads.

Luabind classes, userdata, and C++ callbacks may not expose enough Lua-level state for convenient inspection.

## Visual Studio Lua Debugger Research

The previous research link for Lua debugging is:

- [LuaDkmDebugger](https://github.com/WheretIB/LuaDkmDebugger)
- [LuaDkmDebugger pull request 26](https://github.com/WheretIB/LuaDkmDebugger/pull/26)

Treat this as research material, not a documented XRF-supported debugging workflow.

## Related Pages

- [Debugging engine](../debugging/engine.md)
- [Logs](../debugging/logs.md)
- [Building scripts](../xrf/building/building_scripts.md)
