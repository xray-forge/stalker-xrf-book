# mob_remark

`mob_remark` plays scripted monster animations and optional interaction state. Use it for monster idles, scripted threat
displays, scene beats, and temporary talk/tip control.

## Parameters

### `state`

Type: monster state. Optional. Default: `null`.

Monster state applied on activation.

### `dialog_cond`

Type: condlist. Optional. Default: `null`.

Enables or disables talking based on condlist result.

### `anim`

Type: comma-separated strings. Optional. Default: `null`.

Animation sequence to command.

### `anim_movement`

Type: boolean. Optional. Default: `false`.

Uses movement animation command form when true.

### `anim_head`

Type: string. Optional. Default: `null`.

Parsed and stored; current manager does not use it directly.

### `tip`

Type: string. Optional. Default: `null`.

Tip notification sent once after activation.

### `snd`

Type: string. Optional. Default: `null`.

Parsed and stored; current manager does not play it directly.

### `time`

Type: comma-separated numbers. Optional. Default: `null`.

Per-animation timeout list. Missing values use animation-end condition.

The section also supports common switch fields.

## Runtime behavior

On activation, the manager disables talk, applies monster state, captures the monster, and queues each animation from
`anim`. If a matching `time` value exists, the animation uses a time-end condition. Otherwise it waits for animation
end.

On update, it:

- toggles talk based on `dialog_cond`;
- sends `tip` once through the notification manager;
- sets signal `action_end` once the scripted action is finished.

## Example

```ini
[logic]
active = mob_remark@threat

[mob_remark@threat]
state = threat
anim = stand_idle, attack_prepare
time = 2000, 1000
tip = st_monster_warning
on_signal = action_end | mob_walker@attack
```

## Notes

- `anim` and `time` are parsed as comma-separated lists.
- `action_end` is the usual signal for switching after the scripted remark.
- `noReset` is always set to `true` by the scheme.
