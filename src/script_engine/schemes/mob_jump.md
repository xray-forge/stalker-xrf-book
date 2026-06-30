# mob_jump

`mob_jump` captures a monster, turns it toward a point, and forces a jump. After the jump, it releases the monster from
script control.

Use it for scripted scare jumps and ambush starts.

## Parameters

| Field            | Type           | Required | Default | Description                                                                                                          |
| ---------------- | -------------- | -------- | ------- | -------------------------------------------------------------------------------------------------------------------- |
| `path_jump`      | string         | no       | `null`  | Patrol path whose first point is the jump target base. Relative names are resolved against the active smart terrain. |
| `offset`         | `x,y,z` string | yes      | -       | Offset added to `path_jump` point `0` to form the final jump target.                                                 |
| `ph_jump_factor` | number         | no       | `1.8`   | Jump factor passed to `object.jump`.                                                                                 |
| `on_signal`      | switch field   | yes      | -       | Required by the scheme parser. Usually listens for `jumped`.                                                         |

The section also supports other common switch fields.

## Runtime behavior

On activation, the manager captures the monster and resolves the jump point. It commands the monster to look at the
target, waits for the look action to finish, then calls `object.jump(point, ph_jump_factor)`.

After jumping, it sets signal `jumped` and releases the monster.

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
