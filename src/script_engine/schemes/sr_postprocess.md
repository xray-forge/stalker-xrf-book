# sr_postprocess

`sr_postprocess` applies a gray/noise postprocess effect while the actor is inside a restrictor and applies periodic
radiation and shock hits.

Use it for hazardous visual zones where the actor should see an effect and take damage while inside.

## Parameters

| Field             | Type   | Required | Default | Description                                                                      |
| ----------------- | ------ | -------- | ------- | -------------------------------------------------------------------------------- |
| `intensity`       | number | yes      | -       | Target postprocess intensity. The value is multiplied by `0.01`.                 |
| `intensity_speed` | number | yes      | -       | Ramp speed for entering and leaving the zone. The value is multiplied by `0.01`. |
| `hit_intensity`   | number | yes      | -       | Damage accumulation rate while the actor is inside.                              |

The section also supports common switch fields such as `on_info`, `on_timer`, and actor-zone checks.

## Runtime behavior

On activation, the manager starts a postprocess effector with id `object.id() + 2000`. Each update:

- checks common switch conditions first;
- tests whether the actor is inside the restrictor;
- ramps intensity toward the target when inside and back toward zero when outside;
- updates gray color and noise parameters;
- accumulates hit power while inside;
- once per second, applies radiation and shock hits to the actor.

## Example

```ini
[logic]
active = sr_postprocess@hazard

[sr_postprocess@hazard]
intensity = 40
intensity_speed = 8
hit_intensity = 0.02
on_actor_outside = sr_idle@cooldown
```

## Notes

- `intensity` and `intensity_speed` are percent-style config values.
- Deactivation is not implemented in the current manager and aborts if called.
- The hit direction is zero and impulse is `0`.
