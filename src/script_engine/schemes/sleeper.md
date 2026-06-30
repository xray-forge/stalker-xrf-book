# sleeper

`sleeper` moves a stalker to a sleeping patrol point and then puts the stalker into a sleeping or sitting state. Use it
for beds, camp sleep spots, and scripted rest positions.

## Parameters

### `path_main`

Type: string. Required. Default: none.

Patrol path used to derive both walking and look data. Relative names are resolved against the active smart terrain.

### `wakeable`

Type: boolean. Optional. Default: `false`.

Uses the sitting state instead of the sleeping state when the NPC reaches the final point.

The section also supports common switch fields such as `on_info`, `on_timer`, and `on_signal`.

## Patrol shape

`path_main` must contain either one or two waypoints.

### `1`

The NPC walks to the single point and then sleeps there.

### `2`

The NPC walks using the main path and looks toward the second point when entering the final state.

Any other waypoint count aborts with a config error.

## Example

```ini
[logic]
active = sleeper@bed

[sleeper@bed]
path_main = sleep_place
wakeable = false
on_info = {+alarm_started} walker@wake_up

[walker@wake_up]
path_walk = wake_up_walk
```

## Notes

- `path_main` is required and must exist as a patrol path.
- `wakeable = true` currently maps to `sit`; `wakeable = false` maps to `sleep`.
- The action builds internal `path_walk` and `path_look` data from `path_main`; those are not user-facing fields.
