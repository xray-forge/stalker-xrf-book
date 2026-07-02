# Patrols

Patrol paths are level-authored waypoint paths used by stalker schemes, monster movement, smart cover targets, travel,
spawn helpers, and simulation utilities.

For stalker logic, XRF routes most waypoint behavior through `StalkerPatrolManager`. Schemes such as `walker`,
`sleeper`, `patrol`, and `reach_task` configure the manager with `path_walk`, optional `path_look`, team
synchronization, suggested states, and waypoint callbacks.

## Related schemes

- [walker](./schemes/walker.md)
- [remark](./schemes/remark.md)
- [sleeper](./schemes/sleeper.md)
- [camper](./schemes/camper.md)
- [patrol](./schemes/patrol.md)
- [reach_task](./schemes/reach_task.md)
- [smartcover](./schemes/smartcover.md)

## `path_walk`

`path_walk` is the movement path. The scheme reads it from config, verifies the patrol path exists, parses waypoint
metadata, and sends the object along the path.

Supported waypoint flags include:

| Flag        | Meaning                                                                                       |
| ----------- | --------------------------------------------------------------------------------------------- |
| `a=state`   | Use a state condlist or state value while moving to or through the waypoint.                  |
| `p=percent` | Stop probability at the waypoint. If omitted, the manager uses the normal look-path behavior. |
| `sig=name`  | Set an active scheme signal when the walk waypoint is reached.                                |
| `ret=value` | Pass a numeric return value to a registered patrol callback before animation turn handling.   |

If no `sig` is provided on the last walk waypoint, the manager emits `path_end`.

## `path_look`

`path_look` is an optional look/idle path paired with `path_walk`. The engine uses waypoint flags to choose a matching
look point for a reached walk point.

Supported look flags include:

| Flag         | Meaning                                                                                           |
| ------------ | ------------------------------------------------------------------------------------------------- |
| `a=state`    | Use a state condlist or state value while standing and looking.                                   |
| `t=msec`     | Wait time. `*` means no timeout. Numeric values must be `0` or in the accepted millisecond range. |
| `sig=name`   | Set a signal after turning to the look point. Defaults to `turn_end` when no signal is provided.  |
| `syn`        | Wait for the patrol team before emitting the signal. Requires `sig`.                              |
| `sigtm=name` | Set a signal when the animation-time callback fires.                                              |
| `ret=value`  | Pass a numeric return value to a registered patrol callback after turning.                        |

`syn` is intended for terminal coordination. XRF asserts when it is used on a non-terminal waypoint.

## Example

Button-style interaction that plays a press state, then switches when the timed animation signal is emitted:

```text
path_look waypoint flags: a=press|t=0|sigtm=pressed
logic field: on_signal = pressed | next_scheme@section
```

The exact waypoint flag syntax is stored in level patrol data, not in the LTX file. LTX sections reference the patrol
path names through fields such as `path_walk` and `path_look`.

## Debugging

If a patrol does not work:

- verify `level.patrol_path_exists(path_name)` would pass for `path_walk` and `path_look`;
- check that `path_look` is not the same path as `path_walk`;
- check waypoint flags when look points are not selected;
- check `on_signal` when the movement reaches a point but the scheme does not switch;
- use AI debug overlays and object dumps from the debug panel when the active state is unclear.
