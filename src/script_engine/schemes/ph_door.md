# ph_door

`ph_door` controls a physical door object: initial open or closed state, lock state, NPC locking, tips, sounds, use
handling, and hit-on-bone section switches.

## Parameters

### `closed`

Type: boolean. Optional. Default: `true`.

Activates the door as closed when true, open when false.

### `locked`

Type: boolean. Optional. Default: `false`.

Locks the door and locks it for NPCs.

### `no_force`

Type: boolean. Optional. Default: `false`.

Uses zero joint force instead of applying opening or closing force.

### `not_for_npc`

Type: boolean. Optional. Default: `false`.

Locks the door for NPCs without necessarily locking actor use.

### `show_tips`

Type: boolean. Optional. Default: `true`.

Enables door tip text updates.

### `tip_open`

Type: string. Optional. Default: `tip_door_open`.

Tip shown when the closed door can be opened.

### `tip_close`

Type: string. Optional. Default: `tip_door_close`.

Tip shown when the opened door can be closed.

### `slider`

Type: boolean. Optional. Default: `false`.

Uses slider-style joint angle checks.

### `snd_open_start`

Type: string. Optional. Default: `trader_door_open_start`.

Sound played when opening starts, and when a locked door is used.

### `snd_close_start`

Type: string. Optional. Default: `trader_door_close_start`.

Sound played when closing starts.

### `snd_close_stop`

Type: string. Optional. Default: `trader_door_close_stop`.

Sound played when closing finishes.

### `on_use`

Type: condlist. Optional. Default: `null`.

Switch condlist evaluated when the door is used.

### `hit_on_bone`

Type: bone descriptor list. Optional. Default: empty.

Maps hit bone indexes to section switch condlists.

The implementation currently reads the locked-door tip from `tip_open` with default `tip_door_locked`.

## Use and hit behavior

Using the door evaluates `on_use` and switches to the selected section. Hitting a configured bone evaluates the matching
`hit_on_bone` condlist and switches to the selected section.

`hit_on_bone` uses repeated `bone_index|condlist` descriptors:

```ini
hit_on_bone = 1|ph_door@open %+door_forced%|2|ph_door@broken
```

## Example

```ini
[logic]
active = ph_door@closed

[ph_door@closed]
closed = true
locked = false
on_use = ph_door@open
tip_open = st_open_door

[ph_door@open]
closed = false
on_use = ph_door@closed
tip_close = st_close_door
```

## Notes

- The object is registered as a door for NPCs when the scheme is added.
- The manager expects a physics shell with a `door` joint.
- Deactivation clears the tip text.
- Common switch conditions are checked during update.
