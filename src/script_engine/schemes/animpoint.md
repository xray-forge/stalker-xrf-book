# animpoint

`animpoint` moves a stalker to a registered smart cover point and plays an idle animation there. Use it for traders,
quest NPCs, camp idles, and fixed scene poses where the NPC should stand, sit, or perform an ambient animation at a
known point.

The scheme uses a smart cover record as its anchor. The cover position gives the animation position, and the cover angle
gives the look direction.

## Parameters

| Field              | Type                    | Required | Default             | Description                                                                                                                     |
| ------------------ | ----------------------- | -------- | ------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| `cover_name`       | string                  | no       | `$script_id$_cover` | Registered smart cover name used as the animation anchor.                                                                       |
| `use_camp`         | boolean                 | no       | `true`              | Allows camp manager integration when the animpoint position is inside a camp zone.                                              |
| `reach_movement`   | stalker state           | no       | `walk`              | Movement state used while walking to the animpoint.                                                                             |
| `reach_distance`   | number                  | no       | `0.75`              | Distance threshold for reaching the animpoint. The engine stores it as squared distance.                                        |
| `avail_animations` | comma-separated strings | no       | `null`              | Explicit animation states to choose from. When absent, animations are selected from predicates for the smart cover description. |

The section also supports common switch fields such as `on_info`, `on_timer`, and `on_signal`.

## Usage

Use `animpoint` when the map has a smart cover that represents the desired pose location. The smart cover must be
registered before the scheme starts, otherwise activation aborts when the manager calculates the position.

If `avail_animations` is not set, the engine uses the smart cover description to find compatible animation predicates.
If the description has no registered predicate list, `avail_animations` is required.

With `use_camp = true`, the animpoint can register with a camp manager. Camp roles can choose director or listener
animations from the approved action list.

## Example

```ini
[logic]
active = animpoint@trader

[animpoint@trader]
cover_name = zat_trader_cover
use_camp = false
reach_movement = walk
reach_distance = 1.0
avail_animations = wait, wait_trade
on_info = {+zat_trader_alarm} walker@alarm
```

## Notes

- The smart cover named by `cover_name` must exist in the smart cover registry.
- `avail_animations` is parsed as a comma-separated list.
- The planner uses one action to reach the point and another action to play the selected animation.
- The scheme is interrupted by enemy, anomaly, wounded, abuse, corpse, item, and meet states through common planner
  preconditions.
