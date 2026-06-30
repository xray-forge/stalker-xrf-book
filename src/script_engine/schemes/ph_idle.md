# ph_idle

`ph_idle` is the neutral physical-object scheme. It keeps an object usable or non-usable, shows an optional tip, and can
switch sections when the object is used or hit on configured bones.

Use it for switches, props, doors, breakable objects, and scene objects that should wait for actor use or damage.

## Parameters

| Field              | Type                 | Required | Default      | Description                                            |
| ------------------ | -------------------- | -------- | ------------ | ------------------------------------------------------ |
| `hit_on_bone`      | bone descriptor list | no       | empty        | Maps hit bone indexes to condlists.                    |
| `nonscript_usable` | boolean              | no       | `false`      | Passed to `object.set_nonscript_usable` on activation. |
| `on_use`           | condlist             | no       | `null`       | Switch condlist evaluated when the object is used.     |
| `tips`             | string               | no       | empty string | Tip text assigned to the object.                       |

The section also supports common switch fields such as `on_info` and `on_timer`.

## Bone hit descriptors

`hit_on_bone` uses repeated `bone_index|condlist` descriptors:

```ini
hit_on_bone = 1|ph_idle@hit %+box_was_hit%|2|ph_idle@hit
```

When a matching bone is hit, the manager evaluates the condlist and switches to the selected section.

## Example

```ini
[logic]
active = ph_idle@locked

[ph_idle@locked]
tips = st_locked_box
on_use = {+actor_has_key} ph_idle@open %=play_sound(box_open)%
hit_on_bone = 1|ph_idle@broken %+box_broken%

[ph_idle@open]
nonscript_usable = true
```

## Notes

- The manager clears the tip text on deactivation.
- `on_use` and `hit_on_bone` use explicit section switching.
- Common switch conditions are checked during update.
