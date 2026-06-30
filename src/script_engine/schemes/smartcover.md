# smartcover

`smartcover` makes a stalker use a registered smart cover and update the cover target state while the section is active.
Use it for scripted cover positions, lookout points, and controlled firing from cover.

## Parameters

### `cover_name`

Type: string. Optional. Default: `$script_id$_cover`.

Registered smart cover name.

### `loophole_name`

Type: string. Optional. Default: `null`.

Loophole name stored by the scheme.

### `cover_state`

Type: condlist string. Optional. Default: `default_behaviour`.

Smart cover state selected each update.

### `target_enemy`

Type: story id. Optional. Default: `null`.

Story id of the enemy object to target.

### `target_path`

Type: condlist string. Optional. Default: `nil`.

Condlist selecting a patrol path whose first point becomes the fire target.

### `idle_min_time`

Type: number. Optional. Default: `6`.

Minimum idle time passed to the game object.

### `idle_max_time`

Type: number. Optional. Default: `10`.

Maximum idle time passed to the game object.

### `lookout_min_time`

Type: number. Optional. Default: `6`.

Minimum lookout time passed to the game object.

### `lookout_max_time`

Type: number. Optional. Default: `10`.

Maximum lookout time passed to the game object.

### `exit_body_state`

Type: string. Optional. Default: `stand`.

Exit body state stored by the scheme.

### `use_precalc_cover`

Type: boolean. Optional. Default: `false`.

Stored by the scheme for cover selection compatibility.

### `use_in_combat`

Type: boolean. Optional. Default: `false`.

Allows the combat evaluator to permit smart cover use in combat.

### `weapon_type`

Type: string. Optional. Default: `null`.

Weapon type stored by the scheme.

### `def_state_moving`

Type: stalker state. Optional. Default: `sneak`.

Movement state stored by the scheme.

### `sound_idle`

Type: string. Optional. Default: `null`.

Sound played while the smart cover action executes.

The section also supports common switch fields such as `on_info`, `on_timer`, and `on_signal`.

## Cover states

`cover_state` is parsed as a condlist. The selected value is used to choose smart cover target behavior.

### `idle_target`

Calls idle target mode.

### `lookout_target`

Updates target and calls lookout target mode.

### `fire_target`

Calls fire target mode.

### `fire_no_lookout_target`

Updates target and calls fire-without-lookout mode.

### `default_behaviour` or other values

Updates target and uses default target mode.

### `nil`

Clears the target selector.

When `target_path` selects a patrol path, the first point of that path becomes the smart cover target. If no path is
selected, the action can target `target_enemy` by story id. A stored target position is also supported by the action,
but the current scheme parser does not read a config field for it.

## Signals

When `target_enemy` is set and the stalker is in smart cover, the action updates:

### `enemy_in_fov`

Target enemy is in the current loophole field of view.

### `enemy_not_in_fov`

Target enemy is not in the current loophole field of view.

## Example

```ini
[logic]
active = smartcover@post

[smartcover@post]
cover_name = esc_guard_cover
cover_state = {+alarm_started} fire_target, lookout_target
target_path = esc_guard_fire_point
idle_min_time = 4
idle_max_time = 8
sound_idle = state
on_signal = enemy_in_fov | camper@fire
```

## Notes

- `cover_name` must exist in the smart cover registry when the action initializes.
- `target_path` must resolve to an existing patrol path when it is selected.
- The planner blocks normal ALife while smart cover is needed.
