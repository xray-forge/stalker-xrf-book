# Running Custom Scripts

The engine exposes two development console commands for running Lua from the game console: `run_script` and
`run_string`. They are useful for short investigations on a throwaway save.

These commands are engine debug tools. Availability depends on the engine build.

## Run a Script File

Use `run_script` to run a `.script` file from the game scripts directory:

```text
run_script my_debug_script
```

The engine rescans the scripts path before adding the script to the level script processor. Use this for repeatable
debug snippets that are too large for a single console line.

## Run One Lua Expression

Use `run_string` for a single Lua command:

```text
run_string level.set_weather("default_clear", true)
run_string db.actor:set_actor_position(vector():set(0, 0, 0))
```

The engine preserves argument casing for `run_string`, sends the string to the level script processor when one exists,
and otherwise loads the buffer directly as `@console_command`.

## XRF Helper Examples

Many XRF declarations are available through registered Lua modules after scripts are loaded. For example, debug notes in
spawn configs use command shapes like:

```text
run_string xr_effects.clear_smart_terrain(nil,nil,{"sim_smart_1"})
run_string alife():object(56):set_squad_position(patrol("tst"):point(0))
```

Prefer existing effects and helpers over hand-mutating unrelated manager state.

## Safety

- Test on a disposable save.
- Keep commands small enough to read in logs.
- Use logs or the debug panel to verify the effect after running the command.
- Restart the session if you mutate state that the current manager lifecycle would normally own.
