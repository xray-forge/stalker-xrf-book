# sr_timer

`sr_timer` shows a HUD timer and switches sections when the timer reaches a configured value. Use it for visible
countdowns, elapsed-time displays, evacuation limits, laboratory timers, or mission windows where the player should see
time passing.

The scheme is a restrictor scheme. When activated, it adds the configured HUD static. When deactivated, it removes the
timer static and optional label static.

## Parameters

| Field         | Type                 | Required           | Default       | Description                                                                                                                                |
| ------------- | -------------------- | ------------------ | ------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| `type`        | `inc` or `dec`       | no                 | `inc`         | Timer mode. `inc` counts up from `start_value`; `dec` counts down from `start_value`.                                                      |
| `start_value` | number               | required for `dec` | `0` for `inc` | Starting time in milliseconds.                                                                                                             |
| `on_value`    | `number \| condlist` | no                 | `null`        | Switch when the timer reaches the value. For `dec`, the switch happens at or below the value. For `inc`, it happens at or above the value. |
| `timer_id`    | string               | no                 | `hud_timer`   | HUD custom static id used for the timer text.                                                                                              |
| `string`      | string id            | no                 | `null`        | Optional text string shown in `hud_timer_text`.                                                                                            |

The section also checks common switch fields before updating the timer. If a common switch succeeds, the timer update
for that tick is skipped.

## Usage

Use `type = dec` for deadlines and visible countdowns. It requires `start_value`.

Use `type = inc` for elapsed-time displays. `start_value` is optional and defaults to `0`.

`on_value` is separate from `on_timer`. `on_timer` switches after section activation time; `on_value` switches when the
displayed timer value crosses the configured threshold.

## Example

```ini
[logic]
active = sr_timer@countdown

[sr_timer@countdown]
type = dec
start_value = 60000
timer_id = hud_timer
string = st_lab_countdown
on_value = 0 | sr_idle@failed %+lab_timer_failed%
on_info = {+lab_shutdown_complete} sr_idle@done

[sr_idle@done]

[sr_idle@failed]
```

This section starts a 60-second countdown. It switches to `sr_idle@failed` when the displayed value reaches zero, unless
`lab_shutdown_complete` switches it to `sr_idle@done` first.

## Notes

- Timer values are milliseconds.
- The displayed value is clamped at zero.
- `type` accepts only `inc` and `dec`.
- Decrement timers without `start_value` are invalid.
