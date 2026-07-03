# ph_force

`ph_force` applies a constant force to a physical object toward a patrol point. Use it for scripted pushes, moving
props, and one-shot physical impulses that should last for a configured duration.

## Parameters

### `force`

Type: number. Required. Default: none.

Force magnitude. Must be greater than `0`.

### `time`

Type: number. Required. Default: none.

Duration passed to `object.set_const_force`. Must be greater than `0`.

### `delay`

Type: number. Optional. Default: `0`.

Delay in milliseconds before applying the force.

### `point`

Type: string. Required. Default: none.

Patrol path used to choose the target point.

### `point_index`

Type: number. Optional. Default: `0`.

Patrol point index used as the force target.

The section also supports common switch fields. They are checked before force application.

## Runtime behavior

On activation the scheme validates `force`, `time`, `point`, and `point_index`, then stores the selected patrol point in
state. On manager activation, a non-zero `delay` schedules the first force attempt for a later game time.

Each update first checks common switch conditions. If a switch happens, no force is applied. If processing already
finished, the manager returns. Otherwise it waits for the delay, computes the direction from the physical object to the
stored patrol point, normalizes it, and calls:

```ts
object.set_const_force(direction, force, time);
```

After the call, the manager marks processing complete.

## Example

```ini
[logic]
active = ph_force@push

[ph_force@push]
force = 500
time = 2000
delay = 500
point = push_target
point_index = 0
on_timer = 3000 | ph_idle@done
```

## Notes

- `force`, `time`, and `point` are required.
- `force` and `time` must be positive.
- `point_index` must be inside the patrol path point count.
- The force is applied once. After that, the manager marks processing complete.
- Common switch fields run before force application, so a matching switch can prevent the force entirely.
