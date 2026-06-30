# sr_deimos

`sr_deimos` drives a disorientation effect based on actor movement speed. It ramps intensity, starts postprocess and
looped sounds, can play repeated camera effects, and drains actor health when intensity is high.

## Parameters

### `movement_speed`

Type: number. Optional. Default: `100`.

Target movement speed used to calculate intensity delta.

### `growing_rate`

Type: number. Optional. Default: `0.1`.

Multiplier used when intensity is increasing.

### `lowering_rate`

Type: number. Optional. Default: `growing_rate`.

Multiplier used when intensity is decreasing.

### `pp_effector`

Type: string. Required. Default: none.

Primary postprocess effector name without `.ppe`.

### `pp_effector2`

Type: string. Required. Default: none.

Secondary postprocess effector name without `.ppe`.

### `cam_effector`

Type: string. Required. Default: none.

Camera effector animation name without path or extension.

### `cam_effector_repeating_time`

Type: number. Optional. Default: `10`.

Seconds between repeated camera effects. Stored as milliseconds.

### `noise_sound`

Type: string. Required. Default: none.

Looped noise sound id.

### `heartbeat_sound`

Type: string. Required. Default: none.

Looped heartbeat sound id.

### `health_lost`

Type: number. Optional. Default: `0.01`.

Health amount subtracted when the high-intensity camera effect triggers.

### `disable_bound`

Type: number. Optional. Default: `0.1`.

Intensity below which phase effects are stopped.

### `switch_lower_bound`

Type: number. Optional. Default: `0.5`.

Intensity where heartbeat phase starts or stops.

### `switch_upper_bound`

Type: number. Optional. Default: `0.75`.

Intensity where camera and secondary postprocess can trigger.

The section also supports common switch fields.

## Runtime behavior

The manager compares `movement_speed` with the actor's current movement speed and adjusts intensity between `0` and `1`.
As thresholds are crossed it starts or stops:

- primary postprocess and noise sound;
- heartbeat sound;
- camera effector and secondary postprocess.

When intensity rises above `switch_upper_bound`, the manager may replay the camera effect after
`cam_effector_repeating_time` and subtract `health_lost` from actor health.

When the section switches away, the manager resets related effectors and looped sounds.

## Example

```ini
[logic]
active = sr_deimos@horror

[sr_deimos@horror]
pp_effector = deimos
pp_effector2 = deimos_flash
cam_effector = deimos_camera
noise_sound = deimos_noise
heartbeat_sound = deimos_heartbeat
movement_speed = 80
switch_upper_bound = 0.75
on_info = {+scene_finished} sr_idle@done
```

## Notes

- `cam_effector_repeating_time` is configured in seconds and converted to milliseconds.
- The manager skips updates while the screen is black.
- If an actor binder provides `deimosIntensity`, the manager uses it as the current intensity seed.
