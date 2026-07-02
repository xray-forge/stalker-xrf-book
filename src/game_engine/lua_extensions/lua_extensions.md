# Lua extensions

The engine Lua environment is not plain standalone Lua. OpenXRay-style builds initialize LuaJIT, luabind, engine
exports, standard libraries, and a few engine-specific libraries before game scripts run.

The inspected script engine opens the usual Lua libraries such as `base`, `package`, `table`, `io`, `os`, `math`,
`string`, `bit`, and `ffi`. Debug builds can also expose the Lua `debug` library. LuaJIT is opened unless the executable
starts with `-nojit`.

## Runtime availability

XRF can have TypeScript declarations for a library even when a specific engine executable does not load that library.
Always separate:

- compile-time declarations in `src/typedefs`;
- runtime modules actually opened by the engine;
- modules shipped in the selected gamedata or Lua environment.

This matters for `marshal` and `lfs`: XRF has typings for them, but availability depends on the chosen engine/runtime
package.

## Script path

The engine appends gamedata script paths to `package.path`, allowing scripts to be required from the game script
directory. Keep runtime `require(...)` names aligned with the emitted Lua script layout.
