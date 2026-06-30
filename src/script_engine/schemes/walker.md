# walker

`walker` makes a stalker follow a patrol path while no higher-priority planner state is active. Use it for guards,
ambient movement, scripted walks, and simple station-keeping behavior.

The scheme is a stalker scheme. It adds a walker planner action that runs only while the NPC is alive and not in danger,
combat, anomaly handling, wounded handling, corpse search, item gathering, or abuse reactions.

## Parameters

### `path_walk`

Type: string. Required. Default: none.

Patrol path used for movement. Relative names are resolved against the active smart terrain.

### `path_look`

Type: string. Optional. Default: `null`.

Patrol path used for look points. It must not equal `path_walk`.

### `team`

Type: string. Optional. Default: `null`.

Patrol team name passed to the stalker patrol manager. Relative names are resolved against the active smart terrain.

### `sound_idle`

Type: string. Optional. Default: `null`.

Sound played by the sound manager while the NPC is not in a camp zone.

### `use_camp`

Type: boolean. Optional. Default: `false`.

Allows the NPC to register in a camp story manager when standing inside a camp zone.

### `def_state_standing`

Type: string. Optional. Default: `null`.

Suggested standing animation state.

### `def_state_moving`

Type: string. Optional. Default: `def_state_moving1`.

Suggested moving animation state.

### `def_state_moving1`

Type: string. Optional. Default: `null`.

Compatibility fallback for `def_state_moving`.

The section also supports common switch fields such as `on_info`, `on_signal`, `on_timer`, and actor-distance checks.

## Usage

Use `walker` when one NPC owns its own patrol. Use `patrol` instead when several squad members should share a commander
and follow a formation.

The movement path is parsed the first time the action runs. If `path_look` is present, look waypoints are parsed too.
Waypoint flags and signals are handled by the shared stalker patrol manager.

If `use_camp = true`, the walker action checks whether the NPC is inside a camp zone each update. Inside a camp, the NPC
registers with the camp manager; outside a camp, `sound_idle` can play.

## Example

```ini
[logic]
active = walker@guard

[walker@guard]
path_walk = guard_walk
path_look = guard_look
sound_idle = state
def_state_standing = guard
def_state_moving = walk
on_info = {+zat_b40_alarm} walker@alarm

[walker@alarm]
path_walk = alarm_walk
def_state_moving = run
on_timer = 15000 | walker@guard
```

In a smart terrain named `zat_b40_smart_terrain`, the first section resolves `guard_walk` to
`zat_b40_smart_terrain_guard_walk`.

## Notes

- `path_walk` must exist as a level patrol path.
- `path_look` cannot be the same as `path_walk`.
- The scheme does not force combat behavior. Combat, danger, wounded, and other generic schemes can interrupt it.
