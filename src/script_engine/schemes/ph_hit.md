# ph_hit

`ph_hit` applies a scripted hit to a physical object when the section activates. Use it for one-shot impacts, breaking
props, kicking an object, or driving door and physics reactions through the normal hit API.

The hit is created by `PhysicalHitManager` during activation. The manager does not wait for actor interaction.

## Parameters

### `power`

Type: number. Optional. Default: `0`.

Hit power.

### `impulse`

Type: number. Optional. Default: `1000`.

Hit impulse.

### `bone`

Type: string. Required. Default: none.

Bone name passed to the hit object.

### `dir_path`

Type: string. Required. Default: none.

Patrol path whose first point defines the hit direction.

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

- `bone` and `dir_path` are required. Missing values fail during scheme activation.
- The hit direction is calculated from the object position toward point `0` of `dir_path`.
- The hit type is `strike`.
- The hit is applied on activation. Common switches are checked during later updates.
- Use an `on_timer` or another common switch when the object should move to an idle section after the impact.
