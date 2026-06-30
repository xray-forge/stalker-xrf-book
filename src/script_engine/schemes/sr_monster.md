# sr_monster

`sr_monster` stages a monster ambush from a restrictor. While the actor is inside the zone, it moves a warning sound
source along a patrol path. When the path wraps, it spawns a monster and commands it to run to the path endpoint.

## Parameters

### `snd`

Type: string. Optional. Default: `null`.

Sound id played as the moving warning sound source.

### `delay`

Type: number. Optional. Default: `0`.

Parsed and stored; current manager does not use it directly.

### `idle`

Type: number. Optional. Default: `30`.

Idle duration after the ambush finishes. Multiplied by `10000`.

### `sound_path`

Type: string list. Optional. Default: `null`.

Patrol paths used by the moving warning sound. One path is selected at a time.

### `monster_section`

Type: string. Optional. Default: `null`.

Server object section spawned when the path wraps.

### `slide_velocity`

Type: number. Optional. Default: `7`.

Speed for sliding the warning sound position along the path.

The section also supports common switch fields.

## Runtime behavior

When the actor enters the restrictor, the manager selects a path from `sound_path` and starts sliding a sound position
from point to point. When the selected path wraps back to its start:

1. it spawns `monster_section` at the current sound position;
2. it plays the hard-coded appear sound `monsters_boar_boar_swamp_appear_1`;
3. it captures the spawned monster when it comes online;
4. it commands the monster to run to the final point of the current path;
5. after the monster reaches the final point, it releases and removes the server object;
6. it enters idle state until `idleEnd`.

## Example

```ini
[logic]
active = sr_monster@ambush

[sr_monster@ambush]
snd = monsters_boar_boar_swamp_appear_1
sound_path = ambush_sound_path_1, ambush_sound_path_2
monster_section = boar_normal
slide_velocity = 7
idle = 30
```

## Notes

- `sound_path` should contain patrol paths with enough points for the sound slide and final run target.
- With multiple paths, the manager avoids immediately selecting the same path again.
- The implementation currently stores `delay` but does not apply it in `MonsterManager`.
