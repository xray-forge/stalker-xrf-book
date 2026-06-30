# mob_remark

`mob_remark` plays scripted monster animations and optional interaction state. Use it for monster idles, scripted threat
displays, scene beats, and temporary talk/tip control.

## Parameters

| Field           | Type                    | Required | Default | Description                                                             |
| --------------- | ----------------------- | -------- | ------- | ----------------------------------------------------------------------- |
| `state`         | monster state           | no       | `null`  | Monster state applied on activation.                                    |
| `dialog_cond`   | condlist                | no       | `null`  | Enables or disables talking based on condlist result.                   |
| `anim`          | comma-separated strings | no       | `null`  | Animation sequence to command.                                          |
| `anim_movement` | boolean                 | no       | `false` | Uses movement animation command form when true.                         |
| `anim_head`     | string                  | no       | `null`  | Parsed and stored; current manager does not use it directly.            |
| `tip`           | string                  | no       | `null`  | Tip notification sent once after activation.                            |
| `snd`           | string                  | no       | `null`  | Parsed and stored; current manager does not play it directly.           |
| `time`          | comma-separated numbers | no       | `null`  | Per-animation timeout list. Missing values use animation-end condition. |

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
