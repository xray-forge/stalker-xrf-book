# sr_particle

`sr_particle` plays particle effects from a restrictor section. It supports a simple path-following particle and a
complex mode that plays one particle instance per patrol point.

## Parameters

| Field    | Type       | Required | Default | Description                                                 |
| -------- | ---------- | -------- | ------- | ----------------------------------------------------------- |
| `name`   | string     | yes      | -       | Particle effect name passed to `particles_object`.          |
| `path`   | string     | yes      | -       | Patrol path used by the particle effect. Must not be empty. |
| `mode`   | `1` or `2` | yes      | -       | Particle behavior mode. `1` is simple, `2` is complex.      |
| `looped` | boolean    | no       | `false` | Restarts playback when the particle is not playing.         |

The section also supports common switch fields such as `on_info`, `on_timer`, and `on_signal`.

## Modes

| Mode | Behavior                                                                                                    |
| ---- | ----------------------------------------------------------------------------------------------------------- |
| `1`  | Creates one particle object, loads `path`, starts path playback, and plays it.                              |
| `2`  | Creates one particle object per patrol point and plays each particle at its point after the waypoint delay. |

Complex mode reads waypoint key `d` as delay in milliseconds. Waypoint sound key `s` is currently a development trap and
aborts if present.

## Signals

For non-looped particles, the manager sets `particle_end` after playback has started and all particle objects have
stopped.

```ini
[logic]
active = sr_particle@steam

[sr_particle@steam]
name = anomaly2\steam
path = steam_path
mode = 1
looped = false
on_signal = particle_end | sr_idle@done
```

## Notes

- `mode` accepts only `1` and `2`.
- Deactivation stops all playing particle objects.
- Updates are throttled by the particle scheme update period.
