# sr_cutscene

`sr_cutscene` teleports the actor to a point/look pair, disables game UI, and plays one or more camera effectors. Use it
for scripted first-person scenes and controlled transitions where player input should be temporarily blocked.

## Parameters

### `point`

Type: string. Required. Default: `none`.

Patrol path used by the teleport effect as actor position.

### `look`

Type: string. Required. Default: `none`.

Patrol path used by the teleport effect as actor look target.

### `global_cameffect`

Type: boolean. Optional. Default: `false`.

Marks generated camera effects as global.

### `pp_effector`

Type: string. Optional. Default: `nil`.

Postprocess effector name without `.ppe`. The parser appends `.ppe`.

### `cam_effector`

Type: comma-separated strings. Required. Default: none.

Camera effectors or named effector sets played in order.

### `fov`

Type: number. Optional. Default: `null`.

Field of view stored in the scheme state.

### `enable_ui_on_end`

Type: boolean. Optional. Default: `true`.

Re-enables game UI/input at the end when possible.

### `outdoor`

Type: boolean. Optional. Default: `false`.

Adds a brighten complex effector for outdoor night cutscenes.

The section also supports common switch fields.

## Runtime behavior

On activation, the manager:

1. teleports the actor using `xr_effects.teleport_actor(point, look)`;
2. starts the configured postprocess when it is not `nil`;
3. disables game UI;
4. optionally starts a brighten effector for outdoor night scenes;
5. starts the first configured camera effector or effector set.

Camera progression uses scheme signals. When `cam_effector_stop` is present, the current motion stops and the manager
advances. After the final motion, the manager sets `cameff_end`.

## Example

```ini
[logic]
active = sr_cutscene@intro

[sr_cutscene@intro]
point = intro_actor_point
look = intro_actor_look
cam_effector = intro_camera_1, intro_camera_2
pp_effector = fade_in
enable_ui_on_end = true
on_signal = cameff_end | sr_idle@done
```

## Notes

- `cam_effector` is parsed as a comma-separated list.
- `pp_effector = nil` becomes the nil postprocess constant and is skipped.
- The manager stores the active cutscene object and state in cutscene config while running.
