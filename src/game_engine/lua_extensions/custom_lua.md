# Custom Lua

OpenXRay-style builds use a custom LuaJIT runtime rather than a stock standalone Lua executable. The engine initializes
Lua, opens luabind, registers engine exports, configures script paths, and then loads game scripts.

This matters for XRF code because TypeScriptToLua output runs inside the game script engine, not inside a generic Lua
CLI. Available globals, module search paths, JIT behavior, and engine bindings come from the selected executable.

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

## Notes for XRF scripts

- Treat `src/typedefs` and `xray-16-types` as declarations for what TypeScript can see, then verify ambiguous behavior
  against the engine build.
- Do not assume optional Lua modules such as LFS or marshal are loaded unless the runtime package opens or provides
  them.
- Use engine APIs for game objects, packets, configs, and path resolution. Standalone Lua behavior is a weak reference
  when engine bindings are involved.
