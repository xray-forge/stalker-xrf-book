# mob_jump

`mob_jump` captures a monster, turns it toward a point, and forces a jump. After the jump, it releases the monster from
script control.

Use it for scripted scare jumps and ambush starts.

## Parameters

### `path_jump`

Type: string. Optional. Default: `null`.

Patrol path whose first point is the jump target base. Relative names are resolved against the active smart terrain.

### `offset`

Type: `x,y,z` string. Required. Default: none.

Offset added to `path_jump` point `0` to form the final jump target.

### `ph_jump_factor`

Type: number. Optional. Default: `1.8`.

Jump factor passed to `object.jump`.

### `on_signal`

Type: switch field. Required. Default: none.

Required by the scheme parser. Usually listens for `jumped`.

The section also supports other common switch fields.

## Behavior

On activation, the manager captures the monster and resolves the jump point. It commands the monster to look at the
target, waits for the look action to finish, then calls `object.jump(point, ph_jump_factor)`.

After jumping, it sets signal `jumped` and releases the monster.

## Runtime sequence

1. `SchemeMobJump.activate` requires `on_signal`, reads `path_jump`, `offset`, and `ph_jump_factor`.
2. `MobJumpManager.activate` captures the monster for scripted control.
3. The manager resolves patrol point `0`, adds `offset`, and stores the final jump point.
4. On update, the monster is commanded to look at the jump point.
5. After the look action ends, the manager calls `object.jump(...)`.
6. The manager sets the `jumped` signal and releases the monster.

## Example

```ini
[logic]
active = mob_jump@scare

[mob_jump@scare]
path_jump = bloodsucker_jump
offset = 0, 1, 0
ph_jump_factor = 1.8
on_signal = jumped | mob_home@after_jump
```

## Notes

- `on_signal` must exist in the section.
- `path_jump` must resolve to an existing patrol path.
- `offset` must contain three numeric components.
- If `path_jump` is omitted, the active smart terrain name is used as the path name.
