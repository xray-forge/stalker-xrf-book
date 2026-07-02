# Marshal

`marshal` is a Lua serialization library. XRF has TypeScript declarations for the functions used by the runtime:

- `marshal.encode(value)` converts a Lua value to an encoded representation;
- `marshal.decode(value)` reads an encoded representation back;
- `marshal.clone(value)` creates a cloned value through marshal semantics.

The declarations reference the upstream Lua marshal project: <https://github.com/richardhundt/lua-marshal>.

## Availability

The inspected engine script initialization does not open `marshal` as one of the default Lua libraries. Treat it as an
optional runtime dependency unless the selected executable or gamedata package explicitly provides it.

Guard code that depends on `marshal`, or keep usage in paths where the runtime package is known.

## When to use it

Use marshal only when the runtime really needs Lua-level serialization or cloning. For game save data, prefer the engine
save packet APIs and XRF save/load helpers so the data remains compatible with the engine lifecycle.
