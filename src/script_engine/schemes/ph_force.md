# ph_force

`ph_force` applies a constant force to a physical object toward a patrol point. Use it for scripted pushes, moving
props, and one-shot physical impulses that should last for a configured duration.

## Parameters

| Field         | Type   | Required | Default | Description                                                            |
| ------------- | ------ | -------- | ------- | ---------------------------------------------------------------------- |
| `force`       | number | yes      | -       | Force magnitude. Must be greater than `0`.                             |
| `time`        | number | yes      | -       | Duration passed to `object.set_const_force`. Must be greater than `0`. |
| `delay`       | number | no       | `0`     | Delay in milliseconds before applying the force.                       |
| `point`       | string | yes      | -       | Patrol path used to choose the target point.                           |
| `point_index` | number | no       | `0`     | Patrol point index used as the force target.                           |

The section also supports common switch fields. They are checked before force application.

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
