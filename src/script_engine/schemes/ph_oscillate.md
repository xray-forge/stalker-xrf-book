# ph_oscillate

`ph_oscillate` applies alternating constant force to a physical object joint. Use it for objects that should sway or
rock around a physics bone.

## Parameters

### `joint`

Type: string. Required. Default: none.

Physics joint bone name. Smart terrain prefixing is applied by the parser.

### `period`

Type: number. Required. Default: none.

Time interval used by the oscillation manager.

### `force`

Type: number. Required. Default: none.

Force magnitude used to calculate force growth during the active part of the period.

### `correct_angle`

Type: number. Optional. Default: `0`.

Rotation angle applied to the next force direction when the oscillation flips.

The section also supports common switch fields, parsed into state with the rest of the section.

## Runtime behavior

On activation, the manager:

1. stores the current game time;
2. chooses a random horizontal direction;
3. calculates `force / period`;
4. finds the physics joint by `joint`;
5. starts unpaused.

During update, the manager applies `object.set_const_force(direction, elapsed * force / period, 2)` until `period`
passes. It then flips the horizontal direction, rotates it by `correct_angle`, pauses for half of `period`, and repeats.

## Example

```ini
[logic]
active = ph_oscillate@swing

[ph_oscillate@swing]
joint = door_hinge
period = 1000
force = 20
correct_angle = 15
```

## Notes

- `period` is used directly against `time_global()` deltas, so configure it in the same time units used by engine time.
- The manager looks up the joint on activation. The object must have a physics shell and a matching bone joint.
