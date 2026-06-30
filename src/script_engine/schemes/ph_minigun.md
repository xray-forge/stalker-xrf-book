# ph_minigun

`ph_minigun` controls a physical minigun object. It can aim at a patrol point, the actor, or a story object, and it can
switch sections when a watched target becomes visible or hidden.

## Parameters

| Field                   | Type     | Required | Default  | Description                                                                                                            |
| ----------------------- | -------- | -------- | -------- | ---------------------------------------------------------------------------------------------------------------------- |
| `path_fire`             | string   | no       | `null`   | Patrol path used as the fire point when `target = points`. Smart terrain prefixing is applied.                         |
| `auto_fire`             | boolean  | no       | `false`  | Enables automatic fire when the current target can be hit.                                                             |
| `fire_time`             | number   | no       | `1.0`    | Fire phase duration in seconds.                                                                                        |
| `fire_repeat`           | number   | no       | `0.5`    | Pause duration in seconds between fire phases. `-1` disables the fire/pause timer update.                              |
| `fire_range`            | number   | no       | `50`     | Maximum distance to an enemy target.                                                                                   |
| `target`                | string   | no       | `points` | Fire target. Supported runtime values are `points`, `actor`, or a story object id. Smart terrain prefixing is applied. |
| `track_target`          | boolean  | no       | `false`  | Keeps aiming at the enemy target even when the minigun cannot fire at it.                                              |
| `fire_angle`            | number   | no       | `120`    | Horizontal firing arc used by the manager when checking whether the target can be aimed at.                            |
| `shoot_only_on_visible` | boolean  | no       | `true`   | Requires engine visibility before firing at an enemy target.                                                           |
| `on_target_vis`         | condlist | no       | `null`   | `story_id                                                                                                              | condlist` pair. Switches section when that story object is alive and visible to the minigun.     |
| `on_target_nvis`        | condlist | no       | `null`   | `story_id                                                                                                              | condlist` pair. Switches section when that story object is alive and not visible to the minigun. |

The section also supports common switch fields. They are checked on manager update before minigun-specific processing.

## Runtime behavior

On activation, the manager gets the object's car interface, disables normal script use, clears the tip text, and
activates the mounted weapon if the car has one.

When `target = points`, `path_fire` must point to an existing patrol path. The minigun aims at the first point of that
path. When `target = actor`, the actor is used if alive. Any other non-null value is resolved as a story object id.

Firing only starts when the target is inside `fire_range`, inside the configured firing arc, and visible unless
`shoot_only_on_visible = false`. Enemy target aim height is adjusted for actor, crouching NPCs, wounded NPCs, and normal
standing NPCs.

## Example

```ini
[logic]
active = ph_minigun@post

[ph_minigun@post]
target = actor
auto_fire = true
fire_range = 60
fire_time = 2
fire_repeat = 1
fire_angle = 90
shoot_only_on_visible = true
on_target_nvis = esc_actor_story | ph_idle@quiet
```

## Notes

- `on_target_vis` and `on_target_nvis` use a story object id before the `|`, not a section name.
- If `path_fire` is configured and the patrol path does not exist, activation aborts.
- If the minigun car health reaches zero, the manager stops firing, releases script capture, optionally grants
  `onDeathInfo` from state, and switches the object to `nil` on the next update.
