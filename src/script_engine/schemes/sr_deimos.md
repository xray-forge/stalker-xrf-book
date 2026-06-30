# sr_deimos

`sr_deimos` drives a disorientation effect based on actor movement speed. It ramps intensity, starts postprocess and
looped sounds, can play repeated camera effects, and drains actor health when intensity is high.

## Parameters

| Field                         | Type   | Required | Default        | Description                                                              |
| ----------------------------- | ------ | -------- | -------------- | ------------------------------------------------------------------------ |
| `movement_speed`              | number | no       | `100`          | Target movement speed used to calculate intensity delta.                 |
| `growing_rate`                | number | no       | `0.1`          | Multiplier used when intensity is increasing.                            |
| `lowering_rate`               | number | no       | `growing_rate` | Multiplier used when intensity is decreasing.                            |
| `pp_effector`                 | string | yes      | -              | Primary postprocess effector name without `.ppe`.                        |
| `pp_effector2`                | string | yes      | -              | Secondary postprocess effector name without `.ppe`.                      |
| `cam_effector`                | string | yes      | -              | Camera effector animation name without path or extension.                |
| `cam_effector_repeating_time` | number | no       | `10`           | Seconds between repeated camera effects. Stored as milliseconds.         |
| `noise_sound`                 | string | yes      | -              | Looped noise sound id.                                                   |
| `heartbeat_sound`             | string | yes      | -              | Looped heartbeat sound id.                                               |
| `health_lost`                 | number | no       | `0.01`         | Health amount subtracted when the high-intensity camera effect triggers. |
| `disable_bound`               | number | no       | `0.1`          | Intensity below which phase effects are stopped.                         |
| `switch_lower_bound`          | number | no       | `0.5`          | Intensity where heartbeat phase starts or stops.                         |
| `switch_upper_bound`          | number | no       | `0.75`         | Intensity where camera and secondary postprocess can trigger.            |

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
