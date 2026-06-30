# remark

`remark` plays a short scripted stalker animation, optionally aimed at a target, with optional sound and completion
signals. Use it for scenario beats, one-off gestures, directed looks, and transitions between scripted sections.

## Parameters

### `anim`

Type: condlist. Optional. Default: `wait`.

Animation state selected by condlist when the remark starts.

### `snd`

Type: string. Optional. Default: `null`.

Sound played by the sound manager after the animation when sound is scheduled.

### `snd_anim_sync`

Type: boolean. Optional. Default: `false`.

Controls whether sound is scheduled independently from the animation.

### `target`

Type: string. Optional. Default: `nil`.

Optional look target descriptor.

### `tips`

Type: string. Optional. Default: `null`.

Tip id stored by the scheme.

### `tips_sender`

Type: string. Optional. Default: `null`.

Sender id read only when `tips` is set.

The section also supports common switch fields. `remark` is commonly paired with `on_signal = action_end | ...` or
`on_signal = anim_end | ...`.

## Targets

`target` supports three descriptor forms:

### `story | actor` or `story | story_id`

Looks at the object resolved by story id.

### `path | patrol_path,point_id`

Looks at the selected patrol point.

### `job | job_section,smart_name`

Looks at the object assigned to a smart terrain job.

When `target = nil`, the animation runs without a target descriptor.

## Signals

The remark action sets signals on the active scheme state:

### `anim_end`

The animation callback reaches the sound stage.

### `action_end`

Both animation end and sound end were observed.

The action also observes `sound_end` and `theme_end` signals. Those can be set by sound handling code to allow
`action_end`.

## Example

```ini
[logic]
active = remark@look_actor

[remark@look_actor]
anim = threat_na
target = story|actor
snd = meet_hide_weapon
on_signal = anim_end | walker@guard
```

## Notes

- Invalid `target` descriptors abort with a config error.
- `anim` is a condlist, so it can select different animation states by info portions or conditions.
- The planner blocks normal ALife while the remark section is active.
