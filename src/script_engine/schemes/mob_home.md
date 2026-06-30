# mob_home

`mob_home` assigns a monster to a home area and radius range. Use it to keep monsters near a lair, patrol center, smart
terrain point, or scripted territory.

## Parameters

| Field             | Type          | Required | Default        | Description                                                                                       |
| ----------------- | ------------- | -------- | -------------- | ------------------------------------------------------------------------------------------------- |
| `state`           | monster state | no       | `null`         | Base monster state applied on activation.                                                         |
| `path_home`       | string        | no       | `null`         | Patrol path used as home reference. Relative names are resolved against the active smart terrain. |
| `gulag_point`     | boolean       | no       | `false`        | Uses the monster's smart terrain level vertex as the home point.                                  |
| `aggressive`      | boolean       | no       | `false`        | Passed to `object.set_home`.                                                                      |
| `home_min_radius` | number        | no       | default config | Minimum home radius.                                                                              |
| `home_mid_radius` | number        | no       | midpoint       | Middle home radius. Clamped to the range between min and max.                                     |
| `home_max_radius` | number        | no       | default config | Maximum home radius.                                                                              |

The section also supports common switch fields.

## Runtime behavior

On activation, the manager applies the configured monster state, resolves home parameters, and calls
`object.set_home(home, min, max, aggressive, mid)`.

If `path_home` is set, waypoint data on its first point can provide `minr` and `maxr` values. Explicit `home_min_radius`
and `home_max_radius` override waypoint values.

On deactivation, the manager calls `object.remove_home()`.

## Example

```ini
[logic]
active = mob_home@lair

[mob_home@lair]
path_home = bloodsucker_home
home_min_radius = 5
home_max_radius = 35
aggressive = true
on_info = {+actor_entered_lair} mob_walker@attack
```

## Notes

- `home_max_radius` must be greater than `home_min_radius`.
- With `gulag_point = true`, the home is based on the monster's current smart terrain.
