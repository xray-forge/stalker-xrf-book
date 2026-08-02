# reach_task

`reach_task` drives squad members toward their assigned simulation target. It is part of generic stalker alife behavior,
not a hand-authored active section with LTX parameters.

## Parameters

`reach_task` has no scheme-specific LTX fields in the current TypeScript implementation.

Runtime constants:

### `PATROL_UPDATE_PERIOD`

Value: `1000`.

Milliseconds between movement-order updates.

### `FORMATIONS.back`

Value: built-in formation list.

Default follower offsets behind the squad commander.

## Behavior

`SchemeReachTask.setup()` installs the `SMART_TERRAIN_TASK` evaluator and action inside the nested alife planner. The
evaluator returns true when the NPC's squad has a `REACH_TARGET` action and the assigned simulation target is not yet
reached.

The action initializes movement toward the squad assigned target. The squad commander moves toward the target game
vertex and level vertex. Other squad members follow orders from `ReachTaskPatrolManager`, which keeps them in formation
behind the commander and accelerates members that fall behind.

Objects are removed from the patrol manager when they die or switch offline.

## Runtime sequence

1. `SchemeReachTask.setup` replaces the nested ALife planner's `SMART_TERRAIN_TASK` evaluator and action.
2. `EvaluatorReachedTaskLocation` returns true only when the NPC's squad is doing `REACH_TARGET` and the assigned
   simulation target still reports not reached.
3. `SchemeReachTask.add` subscribes the existing nested `SMART_TERRAIN_TASK` action for scheme events.
4. The action sends the commander toward the assigned target and keeps followers in formation.

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
- Debug this through squad simulation state first; there is normally no `[reach_task]` LTX section to inspect.
