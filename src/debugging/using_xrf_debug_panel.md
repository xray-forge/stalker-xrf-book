# XRF Debug Panel

The XRF debug panel is an in-game CUI dialog for development-only actions. It is available when `forge.ltx` has
`debug.enabled = true`, which is the default project config.

Open the main menu and press `F11`. The panel hides the main menu while it is open. Press `Esc` or the cancel button to
return to the main menu.

## General

The `general` section shows Lua version, LuaJIT state, command-line arguments, and Lua memory usage.

It can also:

- refresh memory usage;
- force Lua garbage collection;
- start or stop Lua profiling;
- print profiling hook stats;
- print manual profiling portion stats;
- toggle simulation debug map overlays;
- dump merged `system.ini` to `_appdata_\\dumps\\system.ltx`;
- dump Lua manager state to `_appdata_\\dumps\\lua_data.json`.

Lua data dumping emits `DUMP_LUA_DATA`, so managers that registered a debug dump callback add their own state to the
JSON payload.

## Commands

The `commands` section exposes checkbox shortcuts for known console toggles.

Boolean commands are written as:

```text
<command> on
<command> off
```

Numeric commands are written as:

```text
<command> 1
<command> 0
```

The list includes AI debug toggles, HUD toggles, `g_god`, `g_unlimitedammo`, `g_autopickup`, and `wpn_aim_toggle`.

## Object

The `object` section works with either the current target object or the nearest game object, depending on the section
checkbox.

It can log:

- scheme state and active section;
- action planner and state planner details;
- state manager details;
- inventory contents;
- relation data.

It can also set the actor relation to the object, kill the object, or set the object wounded. Use these actions on a
throwaway save.

## Items

The `items` section spawns inventory items for the actor. Categories are built from game config sections:

- ammo;
- artefacts;
- consumables;
- detectors;
- helmets;
- outfits;
- weapons.

Ammo spawns as a stack of `30`; other item categories spawn one item.

## Spawn

The `spawn` section spawns creatures and simulation groups.

- `stalkers_list` spawns the selected stalker section near the actor.
- `simulation_group_list` spawns the selected squad section into the nearest smart terrain.

If there is no active game or no suitable smart terrain, the action is skipped and logged.

## Teleport

The `teleport` section lists smart terrains. Selecting one moves the actor to that smart terrain.

If the target game vertex belongs to the current level, the panel sets the actor position directly. If it belongs to a
different level, it calls `game.jump_to_level`. Double-click teleport also closes the main menu after moving the actor.

## Registry and Tasks

The `registry` section lists ALife objects from the simulator, can filter online objects, and can print a registry
summary with manager, scheme, object, smart terrain, event handler, and other collection counts.

The `task` section lists task config sections, can filter active tasks, and can give the selected task through
`TaskManager`.

## Treasures

The `treasures` section shows total, given, and found treasure counts. It can:

- give all treasure coordinates;
- give random treasure coordinates;
- give one selected treasure coordinate;
- teleport to the selected treasure restrictor when it exists.

The section also shows debug details for the selected treasure, including given/checked state, refresh flags, remaining
items, and treasure type.

## Current Shell Sections

`player`, `position`, and `sound` are present as panel sections, but their current TypeScript runtime classes only parse
their form XML and do not bind interactive actions.
