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

## Practical workflow

Start with the generated Lua file that corresponds to the TypeScript module. Confirm that the file exists under
`target/gamedata/scripts` after a script build, then compare the emitted Lua names with the stack trace or log line from
the engine.

Use Lua debugging for runtime-only questions such as callback order, engine object state, and values passed through
luabind. Use Jest and TypeScript tests for parser, manager, scheme, and utility behavior that can be reproduced outside
the engine.

When a Lua debugger cannot inspect userdata, add temporary logging around the TypeScript source and rebuild scripts.
Keep those logs local or remove them before committing documentation or code changes.

## Visual Studio Lua Debugger Research

The previous research link for Lua debugging is:

- [LuaDkmDebugger](https://github.com/WheretIB/LuaDkmDebugger)
- [LuaDkmDebugger pull request 26](https://github.com/WheretIB/LuaDkmDebugger/pull/26)

Treat this as research material, not a documented XRF-supported debugging workflow.

## Related Pages

- [Debugging engine](../debugging/engine.md)
- [Logs](../debugging/logs.md)
- [Building scripts](../xrf/building/building_scripts.md)
