# mob_walker

`mob_walker` makes a monster follow a patrol path and optionally stop at look points for scripted animations, sounds,
and state changes.

Use it for non-combat monster patrols, scripted monster movement, lair idles, and simple ambush staging.

## Parameters

### `path_walk`

Type: string. Required. Default: none.

Patrol path used for monster movement. Relative names are resolved against the active smart terrain.

### `path_look`

Type: string. Optional. Default: `null`.

Patrol path used for look or idle points. It must not equal `path_walk`.

### `state`

Type: monster state. Optional. Default: `null`.

Base monster state read by the shared monster-state parser.

### `no_reset`

Type: boolean. Optional. Default: `false`.

Stored in scheme state for compatibility with monster logic.

The section also supports common switch fields such as `on_info`, `on_signal`, `on_timer`, and actor-zone checks.

## Waypoint behavior

When activated, the manager captures the monster for scripted commands and sends it along `path_walk`.

Movement waypoints can provide extra data parsed from patrol point flags:

### `sig`

Sets a signal on the active scheme state. Use it with `on_signal`.

### `s`

Schedules a sound to play with the next movement or standing command.

### `c`

Uses crouch or steal movement when set to `true`.

### `r`

Uses run movement when set to `true`.

### `b`

Overrides the monster state at that waypoint.

### `flags`

Selects a matching `path_look` point.

Look waypoints can provide:

#### `t`

Standing time in milliseconds.

#### `a`

Animation condlist. The selected value is resolved through the engine `anim` table.

If a movement waypoint has look flags, the manager chooses a matching point from `path_look`, turns the monster toward
it, plays the selected animation, waits, and then resumes movement.

## Example

```ini
[logic]
active = mob_walker@lair

[mob_walker@lair]
path_walk = bloodsucker_walk
path_look = bloodsucker_look
state = nvis
on_signal = attack_ready | mob_home@attack
on_info = {+actor_entered_lair} mob_home@attack

[mob_home@attack]
path_home = bloodsucker_home
```

Use waypoint `sig` data on `bloodsucker_walk` to raise `attack_ready` when the monster reaches the intended patrol
point.

## Notes

- `path_walk` is required.
- `path_look` cannot be the same as `path_walk`.
- The manager reactivates itself if the monster is no longer script-captured.
- If a waypoint requests a look point but no matching look point exists, the scheme aborts with a config error.
