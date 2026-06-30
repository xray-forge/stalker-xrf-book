# camper

`camper` makes a stalker hold a combat position, scan look points, and fire from cover. Use it for ambushes, snipers,
defensive posts, and scripted combat positions.

The scheme owns a combat-camping planner action. It can block regular ALife, item gathering, corpse search, and wounded
helping until close-combat camping is finished.

## Parameters

| Field                      | Type                            | Required | Default               | Description                                                                                                      |
| -------------------------- | ------------------------------- | -------- | --------------------- | ---------------------------------------------------------------------------------------------------------------- |
| `path_walk`                | string                          | yes      | -                     | Patrol path used for movement between camp points. Relative names are resolved against the active smart terrain. |
| `path_look`                | string                          | yes      | -                     | Patrol path used for look and scan points. It must not equal `path_walk`.                                        |
| `sniper`                   | boolean                         | no       | `false`               | Enables sniper scan behavior and sniper update rate.                                                             |
| `no_retreat`               | boolean                         | no       | `false`               | Stored in scheme state. Invalid together with `sniper = true`.                                                   |
| `shoot`                    | `always`, `none`, or `terminal` | no       | `always`              | Controls when the NPC may fire at the visible enemy.                                                             |
| `sniper_anim`              | stalker state                   | no       | `hide_na`             | Sniper animation state stored by the scheme.                                                                     |
| `radius`                   | number                          | no       | `20`                  | Close-combat radius used by the close-combat evaluator.                                                          |
| `def_state_moving`         | stalker state                   | no       | `null`                | Suggested movement state.                                                                                        |
| `def_state_moving_fire`    | stalker state                   | no       | `null`                | Suggested movement-with-fire state.                                                                              |
| `def_state_campering`      | stalker state                   | no       | `null`                | Suggested cover/scanning state.                                                                                  |
| `def_state_standing`       | stalker state                   | no       | `def_state_campering` | Suggested standing state.                                                                                        |
| `def_state_campering_fire` | stalker state                   | no       | `null`                | Suggested cover firing state.                                                                                    |
| `scantime_free`            | number                          | no       | `60000`               | Time to keep scanning without enemy contact before resuming patrol movement.                                     |
| `attack_sound`             | string or `false`               | no       | `fight_attack`        | Sound played when firing. `false` disables it.                                                                   |
| `enemy_idle`               | number                          | no       | `60000`               | Enemy memory timeout before the action stops treating the remembered enemy as active.                            |

The section also supports common switch fields such as `on_info`, `on_timer`, and `on_signal`.

## Shooting modes

| `shoot` value | Behavior                                                     |
| ------------- | ------------------------------------------------------------ |
| `always`      | Fire whenever the enemy is visible and the action can shoot. |
| `none`        | Never fire from this camper action.                          |
| `terminal`    | Fire only from the terminal waypoint of `path_walk`.         |

Any other value aborts with a config error.

## Sniper mode

With `sniper = true`, the action builds a scan table from flags on `path_look`, enables the object's sniper update rate,
and scans look points while the NPC is on a camp patrol walk point.

`sniper = true` cannot be combined with `no_retreat = true`.

## Example

```ini
[logic]
active = camper@ambush

[camper@ambush]
path_walk = ambush_walk
path_look = ambush_look
sniper = true
shoot = terminal
def_state_campering = hide_na
def_state_campering_fire = hide_sniper_fire
attack_sound = fight_attack
on_info = {+ambush_done} walker@after_ambush
```

## Notes

- `path_walk` and `path_look` are both required.
- `path_look` cannot be the same as `path_walk`.
- Danger handling can temporarily override scanning with danger-facing states.
- The implementation uses fixed internal scan constants for enemy dispersion, scan delta, and scan time delta.
