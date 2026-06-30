# danger

`danger` replaces the default danger evaluator for stalkers and updates danger state from heard hostile sounds. It is a
generic planner scheme rather than a normal active movement section.

## Parameters

`danger` has no scheme-specific LTX fields in the current TypeScript implementation.

Runtime constants:

| Constant                              | Value     | Description                                                                              |
| ------------------------------------- | --------- | ---------------------------------------------------------------------------------------- |
| `INERTIA_TIME`                        | `15000`   | Time in milliseconds to keep danger true after the object stops facing a current danger. |
| `BULLET_REACT_DISTANCE_SQR`           | `2 * 2`   | Distance check for nearby bullet-hit sounds.                                             |
| `ALLIES_SHOOTING_ASSIST_DISTANCE_SQR` | `40 * 40` | Distance check for helping allies or reacting to enemy weapon sounds.                    |

## Runtime behavior

The scheme replaces `DANGER` evaluators in the main planner and the nested danger action planner with `EvaluatorDanger`,
then stores a `DangerManager` on state.

The evaluator returns true when the object is facing a danger. If the planner is already running the danger action, it
stores the danger time. When no current danger is faced, it can keep returning true while the last danger time is within
`INERTIA_TIME`.

The manager handles heard sounds. It can set danger time and destination vertex when the NPC hears nearby hostile
bullet-hit sounds, enemy weapon sounds, or ally weapon sounds aimed at an enemy.

## Example

```ini
[logic]
active = walker@guard

[walker@guard]
path_walk = guard_walk
path_look = guard_look
```

`danger` is installed generically for stalkers and does not need to be selected as the active section.

## Notes

- If a stalker is in a smart terrain and faces danger, the evaluator starts the smart terrain alarm.
- The manager ignores heard sounds when the NPC already has a best enemy or cannot select the sound source as an enemy.
