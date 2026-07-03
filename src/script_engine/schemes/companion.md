# companion

`companion` makes a stalker follow and assist the actor. The current implementation uses a simple walking behavior and
planner action rather than a large LTX parameter set.

## Parameters

`companion` has no scheme-specific LTX fields.

The section supports common switch fields. They are parsed into `state.logic`.

## Runtime behavior

On activation, the scheme sets `behavior = 0`, which corresponds to the simple walk behavior in
`ActionCompanionActivity`.

The scheme adds `EActionId.COMPANION_ACTIVITY` to the state planner. It is a stalker scheme, so it expects a stalker
object with a planner and normal movement access.

The action runs when the NPC is alive, has no enemy, and the companion section is active. It:

- clears desired position and direction;
- enables talk;
- picks an accessible assist point near the actor;
- moves to that point using level path movement;
- chooses `raid`, `rush`, or `assault` state based on distance;
- switches to `threat` while standing near the assist point and looking at the actor.

## Example

```ini
[logic]
active = companion@follow

[companion@follow]
on_info = {+companion_stop} walker@wait
```

## Notes

- The source defines additional behavior constants for near/ignore/wait modes, but the current activation code always
  sets simple walking behavior.
- The assist position is chosen to the side of the actor and must be accessible to the NPC.
- Combat or death interrupts the action because the evaluator requires the NPC to be alive and without an enemy.
- Use common switch fields on the companion section to exit follow behavior when quest state changes.
