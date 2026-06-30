# ph_hit

`ph_hit` applies a scripted hit to a physical object when the section activates. Use it for one-shot impacts, breaking
props, kicking an object, or driving door and physics reactions through the normal hit API.

## Parameters

| Field      | Type   | Required | Default | Description                                              |
| ---------- | ------ | -------- | ------- | -------------------------------------------------------- |
| `power`    | number | no       | `0`     | Hit power.                                               |
| `impulse`  | number | no       | `1000`  | Hit impulse.                                             |
| `bone`     | string | yes      | -       | Bone name passed to the hit object.                      |
| `dir_path` | string | yes      | -       | Patrol path whose first point defines the hit direction. |

The section also supports common switch fields such as `on_info` and `on_timer`.

## Example

```ini
[logic]
active = ph_hit@kick

[ph_hit@kick]
power = 0.5
impulse = 1200
bone = door
dir_path = kick_direction
on_timer = 100 | ph_idle@after_hit
```

## Notes

- The hit direction is calculated from the object position toward point `0` of `dir_path`.
- The hit type is `strike`.
- The hit is applied on activation. Common switches are checked during later updates.
