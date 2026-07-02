# cover

`cover` sends a stalker to a nearby cover point around a smart terrain and plays an animation while looking toward a
generated reference position.

## Parameters

### `smart`

Type: string. Required. Default: none.

Smart terrain name used as the center for cover selection.

### `anim`

Type: condlist. Optional. Default: `hide`.

Stalker state condlist used after the NPC reaches cover.

### `sound_idle`

Type: string. Optional. Default: `null`.

Sound alias played while the NPC is in cover.

### `use_attack_direction`

Type: boolean. Optional. Default: `true`.

Parsed into state. The inspected action does not currently read it.

### `radius_min`

Type: number. Optional. Default: `3`.

Minimum random distance from the smart terrain for selecting a cover search point.

### `radius_max`

Type: number. Optional. Default: `5`.

Maximum random distance from the smart terrain for selecting a cover search point.

The section also supports common switch fields.

## Runtime behavior

On activation, the cover action picks a random direction from the smart terrain level vertex and chooses a point between
`radius_min` and `radius_max`. That point is also stored as the look reference. The action asks the engine for the best
cover near it. If no cover is found, it uses the random point itself. If the chosen point is not accessible, it asks the
object for the nearest accessible point.

While moving to cover, the NPC uses the `assault` state. After reaching the cover position, the action resolves `anim`
and sets that stalker state, looking toward the generated enemy-facing position. If `sound_idle` is set, it plays that
sound through the sound manager.

## Example

```ini
[logic]
active = cover@base

[cover@base]
smart = esc_smart_terrain_1
anim = {+under_attack} hide_fire, hide
sound_idle = state
radius_min = 3
radius_max = 6
on_info = {+leave_cover} walker@guard
```

## Notes

- `smart` is required. Activation aborts when it is missing.
- The action blocks normal alife while cover activity is needed.
