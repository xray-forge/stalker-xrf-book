# ph_idle

`ph_idle` is the neutral physical-object scheme. It keeps an object usable or non-usable, shows an optional tip, and can
switch sections when the object is used or hit on configured bones.

Use it for switches, props, doors, breakable objects, and scene objects that should wait for actor use or damage.

## Parameters

### `hit_on_bone`

Type: bone descriptor list. Optional. Default: empty.

Maps hit bone indexes to condlists.

### `nonscript_usable`

Type: boolean. Optional. Default: `false`.

Passed to `object.set_nonscript_usable` on activation.

### `on_use`

Type: condlist. Optional. Default: `null`.

Switch condlist evaluated when the object is used.

### `tips`

Type: string. Optional. Default: empty string.

Tip text assigned to the object.

The section also supports common switch fields such as `on_info` and `on_timer`.

## Bone hit descriptors

`hit_on_bone` uses repeated `bone_index|condlist` descriptors:

```ini
hit_on_bone = 1|ph_idle@hit %+box_was_hit%|2|ph_idle@hit
```

When a matching bone is hit, the manager evaluates the condlist and switches to the selected section.

## Runtime sequence

1. Activation parses common switch fields, `hit_on_bone`, `nonscript_usable`, `on_use`, and `tips`.
2. The object tip text is set immediately.
3. Manager activation calls `object.set_nonscript_usable(...)`.
4. Each update checks common switch fields.
5. `onUse` evaluates `on_use` and switches to the selected section.
6. `onHit` checks the hit bone index against `hit_on_bone` and switches through that condlist when a match exists.

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
- Bone descriptors use engine bone indexes, not bone names.
