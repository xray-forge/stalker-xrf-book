# sr_idle

`sr_idle` is a restrictor scheme that waits and checks switch conditions. It does not run its own effect, movement, UI,
or actor interaction behavior.

Use it as a neutral trigger state when a space restrictor should wait for info portions, timers, actor entry, actor
exit, or other common switch conditions.

## Parameters

`sr_idle` has no scheme-specific parameters. It reads only common switch fields from the section.

| Field                                  | Type                           | Description                                                          |
| -------------------------------------- | ------------------------------ | -------------------------------------------------------------------- |
| `on_info`, `on_info1`, ...             | condlist                       | Switch when the condlist selects another section.                    |
| `on_timer`, `on_timer1`, ...           | `milliseconds \| condlist`     | Switch after the section has been active for the duration.           |
| `on_game_timer`, `on_game_timer1`, ... | `seconds \| condlist`          | Switch after the section has been active for the game-time duration. |
| `on_actor_inside`                      | condlist                       | Switch while the actor is inside the current restrictor.             |
| `on_actor_outside`                     | condlist                       | Switch while the actor is outside the current restrictor.            |
| `on_actor_in_zone`                     | `zone \| condlist`             | Switch while the actor is inside another named zone.                 |
| `on_actor_not_in_zone`                 | `zone \| condlist`             | Switch while the actor is outside another named zone.                |
| `on_npc_in_zone`                       | `story_id \| zone \| condlist` | Switch while the named NPC is inside the named zone.                 |
| `on_npc_not_in_zone`                   | `story_id \| zone \| condlist` | Switch while the named NPC is outside the named zone.                |

## Usage

Use `sr_idle` when the restrictor is only a condition gate:

- wait until the actor enters a volume;
- wait until an info portion is set;
- call an effect through a condlist and then switch;
- hold a trigger in a disabled state until another section enables it.

Because `sr_idle` checks conditions every update, avoid condlists that repeatedly return the same active section while
running effects. Effects in a matching condlist can run every update if the switch itself does not move to another
section.

## Example

```ini
[logic]
active = sr_idle@wait

[sr_idle@wait]
on_actor_inside = sr_idle@inside %=play_sound(alarm_start)%

[sr_idle@inside]
on_actor_outside = sr_idle@wait
on_timer = 10000 | sr_idle@done %+actor_stayed_in_zone%

[sr_idle@done]
```

The first section waits for the actor to enter the restrictor. The second section waits for either actor exit or a
10-second timer.
