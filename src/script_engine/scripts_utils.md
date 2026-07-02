# Utils

Utility modules provide shared, mostly stateless helpers used by binders, schemes, managers, config callbacks, and
server object classes.

They live under `src/engine/core/utils`.

## Main Utility Areas

| Area                                              | Examples                                                     |
| ------------------------------------------------- | ------------------------------------------------------------ |
| `ini`                                             | LTX reads, condlists, switch parsing                         |
| `scheme`                                          | scheme setup, switching, events, object logic initialization |
| `object`                                          | object spawn ini, visual setup, wound/setup helpers          |
| `spawn`                                           | item, ammo, squad, object, and actor-near spawn helpers      |
| `sound`                                           | sound masks, volume console helpers, object sound checks     |
| `time`                                            | time formatting and packet serialization                     |
| `ui`                                              | XML resolution, CUI element setup, screen helpers            |
| `position`, `vector`, `patrol`, `level`, `vertex` | xray spatial helpers                                         |
| `relation`, `community`, `ranks`                  | faction and relationship helpers                             |
| `squad`, `smart_terrain`, `smart_cover`           | simulation and job helpers                                   |
| `logging`                                         | `LuaLogger` and log registry helpers                         |
| `game`                                            | game-flow checks and waiting helpers                         |
| `binding`                                         | global extern registration helpers                           |

## Spawn Helpers

`spawn.ts` wraps common simulator creation paths:

- `spawnItemsForObject`;
- `spawnItemsAtPosition`;
- `spawnAmmoForObject`;
- `spawnAmmoAtPosition`;
- `spawnItemsForObjectFromList`;
- `spawnSquadInSmart`;
- `spawnObject`;
- `spawnObjectInObject`;
- `releaseObject`;
- `spawnCreatureNearActor`.

Ammo helpers respect section `box_size` and split large ammo counts into engine-safe chunks.

## Binding Helpers

`binding.ts` owns `extern` and `getExtern`, which are used by script declarations to expose Lua globals. Use these
helpers instead of assigning globals directly.

## INI and Scheme Helpers

The `ini` helpers parse fields, condlists, string lists, condition lists, timers, switch conditions, and bone
descriptors.

The `scheme` helpers initialize object logic, register scheme constructors, dispatch scheme events, and switch active
sections.

These helpers are the preferred path for LTX-driven behavior. Avoid ad hoc parsing in schemes when a parser already
exists.

## Logging

Use `LuaLogger` for runtime logs. It supports file-aware logging and named log channels used by systems such as
simulation, smart terrain, and psy logic.

## Guidelines

- Keep utilities focused and dependency-light.
- Use existing parser/helpers before adding custom string parsing.
- Put stateful cross-object behavior in managers, not utilities.
- Add tests next to utility modules; most utility folders already have focused Jest tests.
