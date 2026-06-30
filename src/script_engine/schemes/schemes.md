# Schemes

Schemes are script-engine behavior blocks attached to an object through its logic config. A logic section chooses the
first active scheme, and each scheme section describes what the object does until a switch condition moves it to another
section.

```ini
[logic]
active = walker@guard

[walker@guard]
path_walk = guard_walk
path_look = guard_look
on_info = {+alarm_started} walker@alarm

[walker@alarm]
path_walk = alarm_walk
def_state_moving = run
```

The scheme name is the part before the suffix. For example, `walker@guard` uses the `walker` implementation, and
`sr_idle@wait_for_actor` uses the `sr_idle` implementation.

## Section switching

Many active scheme sections support the same switching fields. The engine parses them from the active section and checks
them in the order used by the scheme switch parser. Numbered variants such as `on_info1` and `on_info2` let one section
define several checks of the same kind.

### `on_info`, `on_info1`, ...

Value shape: condlist.

When the condlist picks a target section.

### `on_signal`, `on_signal1`, ...

Value shape: `signal | condlist`.

When a waypoint or manager sets the named signal.

### `on_timer`, `on_timer1`, ...

Value shape: `milliseconds | condlist`.

After the section has been active for the given real-time duration.

### `on_game_timer`, `on_game_timer1`, ...

Value shape: `seconds | condlist`.

After the section has been active for the given game-time duration.

### `on_actor_inside`

Value shape: condlist.

When the actor is inside the current restrictor object.

### `on_actor_outside`

Value shape: condlist.

When the actor is outside the current restrictor object.

### `on_actor_in_zone`

Value shape: `zone | condlist`.

When the actor is inside the named zone.

### `on_actor_not_in_zone`

Value shape: `zone | condlist`.

When the actor is outside the named zone.

### `on_npc_in_zone`

Value shape: `story_id | zone | condlist`.

When the NPC resolved by story id is inside the named zone.

### `on_npc_not_in_zone`

Value shape: `story_id | zone | condlist`.

When the NPC resolved by story id is outside the named zone.

### `on_actor_dist_le`

Value shape: `distance | condlist`.

When the object sees the actor and actor distance is less than or equal to the value.

### `on_actor_dist_le_nvis`

Value shape: `distance | condlist`.

Same distance check, without requiring actor visibility.

### `on_actor_dist_ge`

Value shape: `distance | condlist`.

When the object sees the actor and actor distance is greater than the value.

### `on_actor_dist_ge_nvis`

Value shape: `distance | condlist`.

Same distance check, without requiring actor visibility.

A condlist can also set info portions or run effects while selecting the next section:

```ini
on_info = {+actor_has_key} ph_door@open %=play_sound(door_unlock)%
```

If the condlist returns an empty section, `nil`, or the current section, the switch is skipped.

`nil` is a special inactive target, not a registered scheme implementation. Switching helpers use it to clear active
scheme state or skip activation in places where a real section would normally be expected.

## Scheme families

### Stalker schemes

Examples: `walker`, `patrol`, `remark`, `animpoint`, `smartcover`.

Human NPC movement, idle states, combat support, meetings, and scripted actions.

### Monster schemes

Examples: `mob_walker`, `mob_home`, `mob_remark`, `mob_combat`.

Monster movement, territory, scripted animations, and combat hooks.

### Restrictor schemes

Examples: `sr_idle`, `sr_timer`, `sr_teleport`, `sr_particle`, `sr_cutscene`.

Trigger volumes, timers, effects, postprocess, and actor-facing scripted events.

### Physical schemes

Examples: `ph_idle`, `ph_button`, `ph_door`, `ph_code`, `ph_on_hit`.

Scripted behavior for physical objects and usable scene objects.

### Helicopter schemes

Examples: `heli_move`.

Helicopter patrol, movement, targeting, and attack behavior.

### Generic schemes

Examples: `combat`, `danger`, `death`, `hit`, `meet`, `post_combat_idle`, `wounded`.

Shared behavior that can be enabled alongside active stalker or monster logic.

## Patrol names

Several schemes read patrol path fields such as `path_walk` and `path_look`. When an object is running under a smart
terrain, relative path names are resolved against the smart terrain name. For example, `path_walk = guard_walk` in smart
terrain `zat_b40_smart_terrain` resolves to `zat_b40_smart_terrain_guard_walk`.

Use full path names when the path does not belong to the active smart terrain.

## Common checks

- `path_walk` is usually required for movement schemes.
- `path_look` must not be the same path as `path_walk`.
- Scheme sections can have suffixes, such as `walker@start`, `walker@alarm`, or `sr_timer@lab_countdown`.
- For timed transitions, section activation time is reset when switching to a different section.
- For switch-heavy logic, prefer `sr_idle` when the object should do nothing except wait for conditions.
