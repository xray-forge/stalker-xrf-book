# camper

`camper` makes a stalker hold a combat position, scan look points, and fire from cover. Use it for ambushes, snipers,
defensive posts, and scripted combat positions.

The scheme owns a combat-camping planner action. It can block regular ALife, item gathering, corpse search, and wounded
helping until close-combat camping is finished.

## Parameters

### `path_walk`

Type: string. Required. Default: none.

Patrol path used for movement between camp points. Relative names are resolved against the active smart terrain.

### `path_look`

Type: string. Required. Default: none.

Patrol path used for look and scan points. It must not equal `path_walk`.

### `sniper`

Type: boolean. Optional. Default: `false`.

Enables sniper scan behavior and sniper update rate.

### `no_retreat`

Type: boolean. Optional. Default: `false`.

Stored in scheme state. Invalid together with `sniper = true`.

### `shoot`

Type: `always`, `none`, or `terminal`. Optional. Default: `always`.

Controls when the NPC may fire at the visible enemy.

### `sniper_anim`

Type: stalker state. Optional. Default: `hide_na`.

Sniper animation state stored by the scheme.

### `radius`

Type: number. Optional. Default: `20`.

Close-combat radius used by the close-combat evaluator.

### `def_state_moving`

Type: stalker state. Optional. Default: `null`.

Suggested movement state.

### `def_state_moving_fire`

Type: stalker state. Optional. Default: `null`.

Suggested movement-with-fire state.

### `def_state_campering`

Type: stalker state. Optional. Default: `null`.

Suggested cover/scanning state.

### `def_state_standing`

Type: stalker state. Optional. Default: `def_state_campering`.

Suggested standing state.

### `def_state_campering_fire`

Type: stalker state. Optional. Default: `null`.

Suggested cover firing state.

### `scantime_free`

Type: number. Optional. Default: `60000`.

Time to keep scanning without enemy contact before resuming patrol movement.

### `attack_sound`

Type: string or `false`. Optional. Default: `fight_attack`.

Sound played when firing. `false` disables it.

### `enemy_idle`

Type: number. Optional. Default: `60000`.

Enemy memory timeout before the action stops treating the remembered enemy as active.

The section also supports common switch fields such as `on_info`, `on_timer`, and `on_signal`.

## Shooting modes

### `always`

Fire whenever the enemy is visible and the action can shoot.

### `none`

Never fire from this camper action.

### `terminal`

Fire only from the terminal waypoint of `path_walk`.

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
