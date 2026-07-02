# Custom Lua

OpenXRay-style builds use a custom LuaJIT runtime rather than a stock standalone Lua executable. The engine initializes
Lua, opens luabind, registers engine exports, configures script paths, and then loads game scripts.

## Libraries opened by the engine

The inspected script engine opens standard Lua libraries and LuaJIT-related libraries used by game scripts:

- `package`, `table`, `io`, `os`, `math`, and `string`;
- `bit` and `ffi`;
- LuaJIT support unless `-nojit` is passed;
- `debug` in non-master/debug-capable builds;
- engine-specific helpers such as `xrluafix`;
- Tracy Lua integration when compiled into the engine.

Do not assume every fork opens the same set. Check the selected executable if a script depends on a non-standard module.

## Useful flags

`-nojit` disables LuaJIT JIT compilation. This can make some debugging sessions easier, but it also changes profiler
behavior.

`-dump_bindings` asks the script engine to dump binding information. Use it when comparing what the engine exported with
what the TypeScript declarations say exists.
