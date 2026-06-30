# help_wounded

`help_wounded` is generic stalker behavior for helping nearby wounded friendly stalkers. It sends a suitable NPC to the
wounded target and plays the medkit-help animation.

## Parameters

### `help_wounded_enabled`

Type: boolean. Optional. Default: `true`.

Enables or disables wounded-helper behavior for the current reset section.

Runtime constants:

### `DISTANCE_TO_HELP`

Value: `30`.

Maximum search distance for wounded targets.

### `HELPING_WOUNDED_OBJECT_KEY`

Value: `helping_wounded_object`.

Portable-store key used to reserve a wounded target for one helper.

## Runtime behavior

The scheme adds `IS_WOUNDED_EXISTING` and a `HELP_WOUNDED` action. The evaluator only allows helping when the NPC is
alive, has no enemy, is not zombied, is not wounded, is not the cinematic actor visual, and a wounded target is nearby.

When a target is selected, the evaluator stores the wounded object id, vertex id, and position in scheme state, and
marks the target in portable storage so another helper does not claim it. The action runs to the target, then switches
to `help_wounded_with_medkit`, looks at the wounded position, and plays `wounded_medkit` once.

The `finishObjectHelpWounded` helper gives the selected wounded object a medkit and unlocks its wounded manager medkit
use.

## Example

```ini
[logic]
active = walker@camp

[walker@camp]
path_walk = camp_walk
path_look = camp_look
help_wounded_enabled = false
```

## Notes

- The helper action blocks item gathering and alife actions while a wounded target is selected.
- Finalizing the action frees the wounded target reservation.
