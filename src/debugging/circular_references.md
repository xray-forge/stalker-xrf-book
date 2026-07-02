# Circular References

Circular references usually show up in XRF as initialization problems: a manager asks for another manager while modules
are still loading, or a debug dump walks a graph of tables that point back to each other.

## Manager Resolution

Use `getManager(ManagerClass)` for normal manager access. It initializes the manager on first use and stores it in the
runtime registry.

Use `getManagerByName("ManagerName")` only when importing the manager class would create a circular module dependency.
It returns an already initialized manager by name and cannot create one by itself.

```ts
const surgeManager: SurgeManager = getManagerByName("SurgeManager") as SurgeManager;
```

If the result can be missing during startup, handle `null` instead of assuming the manager exists.

## Resolve Logging

`forge.ltx` has a debug flag for logger creation:

```ini
[debug]
resolve_log_enabled = true
```

When enabled, each `LuaLogger` prints a `Declared logger` message when it is created. This is noisy, but useful when you
need to see which modules are loaded before a crash or circular dependency failure.

Turn it back off after the investigation:

```ini
[debug]
resolve_log_enabled = false
```

## Dumping Circular Data

The XRF JSON helper used by debug dumps is circular-reference aware. When it sees a table it has already visited, it
writes:

```json
"<circular_reference>"
```

When the dump exceeds the configured depth limit, it writes:

```json
"<depth_limit>"
```

Use the debug panel `general` section to dump Lua state to `_appdata_\\dumps\\lua_data.json`, then search for these
markers to find strongly connected runtime state.

## Practical Checks

- Move type-only imports to `import type` when the dependency is only needed by TypeScript.
- Prefer event callbacks or weak manager lookup when two managers need to observe each other.
- Avoid top-level runtime work in modules that are imported by many systems.
- Keep debug dumps bounded; do not serialize raw engine userdata.
