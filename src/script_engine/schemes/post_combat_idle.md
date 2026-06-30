# post_combat_idle

`post_combat_idle` makes a non-zombied stalker wait briefly after combat before returning to alife, looting, or helper
behavior. It is installed by setup code and does not have a hand-authored active section.

## Parameters

`post_combat_idle` has no scheme-specific LTX fields in the current TypeScript implementation.

The wait duration can be affected by resolved logic overrides stored on the object registry:

| Override            | Default | Description                                                                 |
| ------------------- | ------- | --------------------------------------------------------------------------- |
| `minPostCombatTime` | `5`     | Minimum randomized wait time in seconds after a non-actor enemy disappears. |
| `maxPostCombatTime` | `10`    | Maximum randomized wait time in seconds after a non-actor enemy disappears. |

These overrides are consumed by the evaluator. Their parsing is handled outside `SchemePostCombatIdle`.

## Runtime behavior

`SchemePostCombatIdle.setup()` skips zombied-community stalkers. For other stalkers it:

1. creates `post_combat_idle` state in the object registry;
2. replaces `ENEMY` evaluators in the main planner and nested combat planner;
3. adds the `POST_COMBAT_WAIT` action to the combat planner.

The evaluator returns true while a selectable best enemy exists. When the enemy disappears, it starts a timer. Actor
targets reset the timer to the current time. Other enemies use a randomized delay between `minPostCombatTime` and
`maxPostCombatTime`, or the default `5` to `10` seconds.

The wait action equips the best weapon, sets danger/crouch/stand posture, uses danger sight, starts the `hide` animation
when possible, and plays `post_combat_wait`. On finalize, it plays `post_combat_relax` and clears the animation state.

## Example

```ini
[logic]
active = walker@guard

[walker@guard]
path_walk = guard_walk
path_look = guard_look
```

`post_combat_idle` is installed by the stalker scheme setup path. It is not selected with `[logic] active`.

## Notes

- The action does not start the hide animation while the NPC is in a smart cover or its weapon is locked.
- If an animation is still clearing after the timer expires, the evaluator can keep returning true until the animation
  marker is gone.
