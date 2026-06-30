# heli_move

`heli_move` moves a helicopter along a patrol path and configures its targeting, weapon ranges, engine sound, fire
trail, and optional combat health UI.

## Parameters

### `path_move`

Type: string. Required. Default: none.

Patrol path used for helicopter movement. Must exist.

### `path_look`

Type: string. Optional. Default: `null`.

Patrol path used as a look point, or `actor` to keep looking at the actor.

### `enemy`

Type: string. Optional. Default: `null`.

Enemy preference passed to the fire manager. Runtime handles `actor`, `all`, `nil`, or a story id.

### `fire_point`

Type: string. Optional. Default: `null`.

Patrol path whose first point is used as a fallback fire point.

### `max_velocity`

Type: number. Required. Default: none.

Maximum helicopter movement velocity.

### `max_mgun_attack_dist`

Type: number. Optional. Default: `null`.

Overrides helicopter max minigun attack distance.

### `min_mgun_attack_dist`

Type: number. Optional. Default: `null`.

Overrides helicopter min minigun attack distance.

### `max_rocket_attack_dist`

Type: number. Optional. Default: `null`.

Overrides helicopter max rocket attack distance.

### `min_rocket_attack_dist`

Type: number. Optional. Default: `null`.

Overrides helicopter min rocket attack distance.

### `upd_vis`

Type: number. Optional. Default: `10`.

Visibility refresh interval passed to the fire manager, in seconds.

### `use_rocket`

Type: boolean. Optional. Default: `true`.

Enables rocket use during attack.

### `use_mgun`

Type: boolean. Optional. Default: `true`.

Enables minigun use during attack.

### `engine_sound`

Type: boolean. Optional. Default: `true`.

Enables helicopter engine sound.

### `stop_fire`

Type: boolean. Optional. Default: `false`.

With `path_look = actor`, holds the helicopter position while the actor is visible.

### `show_health`

Type: boolean. Optional. Default: `false`.

Shows the helicopter combat health UI while active.

### `fire_trail`

Type: boolean. Optional. Default: `false`.

Enables the helicopter fire trail effect.

### `invulnerable`

Type: boolean. Optional. Default: `false`.

Sets object registry invulnerable state.

### `immortal`

Type: boolean. Optional. Default: `false`.

Sets object registry immortal state.

### `mute`

Type: boolean. Optional. Default: `false`.

Sets object registry mute state.

The section also supports common switch fields. They are checked before movement updates.

## Runtime behavior

On activation, the manager asserts that `path_move` exists, parses waypoint data, creates the movement patrol, and sets
linear acceleration and max velocity from `max_velocity`. It loops through patrol points and records waypoint signals
from parsed waypoint data into `state.signals`.

If `path_look` is set to `actor`, the look point is refreshed from the actor position on update. If it names a patrol
path, the first point of that path is used as the look point. The manager blocks free look and applies the look target
through the helicopter fly manager.

Weapon distance fields are written to the engine helicopter object when present. `use_mgun` and `use_rocket` update the
engine attack flags. The fire manager uses `enemy`, `fire_point`, and `upd_vis` to select or refresh enemies.

## Example

```ini
[logic]
active = heli_move@patrol

[heli_move@patrol]
path_move = esc_heli_move
path_look = actor
max_velocity = 30
enemy = actor
use_mgun = true
use_rocket = false
upd_vis = 5
show_health = true
on_signal = patrol_done | heli_move@return
```

## Notes

- `path_move` is required and must exist.
- When `path_look` names a patrol path, that patrol path must exist.
- `fire_point` is read as a patrol path name and the first point is used. The current activation code does not assert
  that the path exists before constructing the patrol.
- On save/load, the manager stores movement state, last and next waypoint indices, and whether a waypoint callback was
  pending.
