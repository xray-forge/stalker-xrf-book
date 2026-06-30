# sr_teleport

`sr_teleport` teleports the actor after the actor enters a restrictor and a timeout elapses. It can choose from up to
ten weighted destination/look pairs.

## Parameters

### `timeout`

Type: number. Optional. Default: `900`.

Delay in milliseconds between actor entry and teleport.

### `point1` ... `point10`

Type: string. Required: at least one pair. Default: `none`.

Patrol path whose first point is the teleport position.

### `look1` ... `look10`

Type: string. Required: at least one pair. Default: `none`.

Patrol path whose first point defines look direction after teleport.

### `prob1` ... `prob10`

Type: number. Optional. Default: `100`.

Weight for the matching point/look pair.

The section also supports common switch fields. They are checked after teleport processing when the manager is idle.

## Runtime behavior

When the actor enters the restrictor, the manager:

1. switches from idle to activated state;
2. starts the teleport postprocess effector;
3. waits `timeout` milliseconds;
4. chooses a destination by subtracting weights from a random value in the total probability range;
5. teleports the actor to `pointN[0]` and looks toward `lookN[0] - pointN[0]`;
6. returns to idle state.

The parser stops reading destination pairs when it finds `pointN = none` or `lookN = none`.

## Example

```ini
[logic]
active = sr_teleport@burnt_farm

[sr_teleport@burnt_farm]
timeout = 1000
point1 = teleport_walk_a
look1 = teleport_look_a
prob1 = 25
point2 = teleport_walk_b
look2 = teleport_look_b
prob2 = 75
```

## Notes

- At least one complete `pointN` and `lookN` pair is required.
- `probN` is a weight, not a normalized percentage.
- The teleport triggers again if the actor remains or re-enters after the manager returns to idle.
