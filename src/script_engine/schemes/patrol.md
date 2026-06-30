# patrol

`patrol` coordinates a group of stalkers around a commander. Use it when several NPCs should move as one patrol instead
of each running an independent `walker` path.

The scheme is a stalker scheme. It registers each participating object in a shared patrol manager. The first registered
object becomes commander unless a section has `commander = true`.

## Parameters

### `path_walk`

Type: string. Required. Default: none.

Commander movement path. Relative names are resolved against the active smart terrain.

### `path_look`

Type: string. Optional. Default: `null`.

Optional look path for the commander. It must not equal `path_walk`.

### `formation`

Type: string. Optional. Default: `back`.

Formation used by followers. Supported values are defined by the patrol formation config.

### `silent`

Type: boolean. Optional. Default: `false`.

Disables automatic patrol movement sounds.

### `move_type`

Type: string. Optional. Default: `patrol`.

Stored movement type for patrol logic compatibility.

### `commander`

Type: boolean. Optional. Default: `false`.

Marks this NPC as the patrol commander when registering in the patrol manager.

### `def_state_standing`

Type: string. Optional. Default: `null`.

Suggested standing animation state.

### `def_state_moving`

Type: string. Optional. Default: `def_state_moving1`.

Suggested commander moving animation state.

### `def_state_moving1`

Type: string. Optional. Default: `null`.

Compatibility fallback for `def_state_moving`.

The section also supports common switch fields such as `on_info`, `on_signal`, `on_timer`, and actor-distance checks.

## Usage

Give all members of the same patrol the same `path_walk`. If the NPCs belong to a squad, the engine keys the shared
patrol manager by path name plus squad id, so separate squads can use the same route without sharing one runtime
manager.

Followers update their target roughly once per second. They follow the commander's current path, direction, movement
state, and formation offset. If a follower falls too far behind, the patrol manager can return an accelerated movement
state based on the commander's current state.

The commander can change formation from waypoint callback return values:

### `0`

`line`

### `1`

`around`

### `2`

`back`

## Example

```ini
[logic]
active = patrol@route

[patrol@route]
path_walk = squad_patrol_walk
path_look = squad_patrol_look
formation = back
commander = true
def_state_moving = patrol
on_info = {+base_alarm} patrol@alarm

[patrol@alarm]
path_walk = squad_alarm_walk
formation = line
silent = true
def_state_moving = rush
```

Use the same section on the intended patrol members. Set `commander = true` on the NPC that should drive the formation.

## Notes

- `path_walk` is required.
- `path_look` cannot be the same as `path_walk`.
- A patrol manager rejects attempts to register more than seven objects.
- Objects unregister from the patrol manager on scheme deactivation, death, or offline switch.
