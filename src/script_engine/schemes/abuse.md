# abuse

`abuse` is a generic stalker scheme that makes an NPC react when the actor abuses it repeatedly. The current action
reaction is a punch animation aimed at the actor.

## Parameters

`abuse` has no scheme-specific LTX fields in the current TypeScript implementation.

Runtime values are stored in `AbuseManager`:

| Runtime value    | Default | Description                                           |
| ---------------- | ------- | ----------------------------------------------------- |
| `isEnabled`      | `true`  | Enables or disables abuse accumulation.               |
| `abuseRate`      | `2`     | Multiplier used when abuse is added.                  |
| `abuseThreshold` | `5`     | Threshold at which the evaluator reports abuse.       |
| `abuseValue`     | `0`     | Current accumulated abuse value. It decays over time. |

## Runtime behavior

The scheme adds an `IS_ABUSED` evaluator and an abuse action to the stalker planner. The action can run only while the
NPC is alive, not in danger, and not wounded. When selected, it clears desired position and direction and sets the NPC
state to `punch`, looking at the actor.

`AbuseManager.update()` decays accumulated abuse over time, clamps it near the threshold, and returns whether the value
is currently above the threshold.

## Example

```ini
[logic]
active = walker@idle

[walker@idle]
path_walk = guard_walk
path_look = guard_look
```

`abuse` is installed as a generic stalker scheme. It is not normally selected as the active section in `[logic]`.

## Notes

- The public manager API exposes `addAbuse`, `clearAbuse`, `enableAbuse`, `disableAbuse`, and `setAbuseRate`.
- The page documents the current engine behavior. It does not define a separate config field for changing the abuse
  threshold or rate from LTX.
