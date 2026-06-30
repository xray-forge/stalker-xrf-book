# ph_button

`ph_button` plays a button animation and switches sections when the object is used. Use it for physical buttons, levers,
and scripted controls.

## Parameters

### `anim`

Type: string. Required. Default: none.

Animation cycle played on activation.

### `anim_blend`

Type: boolean. Optional. Default: `true`.

Passed as the blending flag to `object.play_cycle`.

### `on_press`

Type: condlist. Optional. Default: `null`.

Switch condlist evaluated when the active button is used.

### `tooltip`

Type: string. Optional. Default: `null`.

Tip text shown on the object. Empty text is used when absent.

The section also supports common switch fields such as `on_info` and `on_timer`.

## Example

```ini
[logic]
active = ph_button@off

[ph_button@off]
anim = idle_off
tooltip = st_press_button
on_press = ph_button@on %+button_pressed%

[ph_button@on]
anim = idle_on
anim_blend = false
```

## Notes

- `anim` is required.
- `on_press` only runs when the object is still in the active button section.
- Common switch conditions are checked during update.
