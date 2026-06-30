# reach_task

`reach_task` drives squad members toward their assigned simulation target. It is part of generic stalker alife behavior,
not a hand-authored active section with LTX parameters.

## Parameters

`reach_task` has no scheme-specific LTX fields in the current TypeScript implementation.

Runtime constants:

| Constant               | Value                   | Description                                          |
| ---------------------- | ----------------------- | ---------------------------------------------------- |
| `PATROL_UPDATE_PERIOD` | `1000`                  | Milliseconds between movement-order updates.         |
| `FORMATIONS.back`      | built-in formation list | Default follower offsets behind the squad commander. |

## Runtime behavior

`SchemeReachTask.setup()` installs the `SMART_TERRAIN_TASK` evaluator and action inside the nested alife planner. The
evaluator returns true when the NPC's squad has a `REACH_TARGET` action and the assigned simulation target is not yet
reached.

The action initializes movement toward the squad assigned target. The squad commander moves toward the target game
vertex and level vertex. Other squad members follow orders from `ReachTaskPatrolManager`, which keeps them in formation
behind the commander and accelerates members that fall behind.

Objects are removed from the patrol manager when they die or switch offline.

## Example

```ini
[logic]
active = walker@idle

[walker@idle]
path_walk = idle_walk
path_look = idle_look
```

`reach_task` is driven by squad simulation state. It is not normally configured with a dedicated `[reach_task]` section.

## Notes

- Movement switches between game-path and level-path movement depending on whether the commander is on the target game
  vertex.
- During surge, reach-task movement uses running with free mental state.
