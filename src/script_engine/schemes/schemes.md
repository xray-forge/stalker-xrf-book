# Schemes

A scheme is the behavior selected by an object's logic configuration. `[logic] active` names the initial section; the
part before `@` selects the implementation.

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

Digits are ignored while resolving a scheme name, so numbered variants share the same implementation.

## Switching sections

Active sections can define switch rules. The engine evaluates rule groups in this fixed order: actor distance, signals,
info portions, real-time timers, game-time timers, actor-zone rules, then NPC-zone rules. Within a group, rules follow
their order in the section. Numbered variants such as `on_info1` add another rule of the same type.

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

An empty target, `nil`, or the current section does not switch. Timer baselines reset after a successful switch.

## Scheme families

- **Stalker:** NPC movement, animations, combat, and interaction (`walker`, `remark`, `animpoint`, `smartcover`).
- **Monster:** monster movement, territory, animations, and combat (`mob_walker`, `mob_home`, `mob_remark`,
  `mob_combat`).
- **Restrictor:** zone triggers, timers, visual effects, and actor events (`sr_idle`, `sr_timer`, `sr_teleport`,
  `sr_particle`).
- **Physical:** usable and reactive world objects (`ph_idle`, `ph_button`, `ph_door`, `ph_code`, `ph_on_hit`).
- **Helicopter:** scripted flight and weapons (`heli_move`).
- **Generic:** behavior attached alongside an active scheme (`combat`, `danger`, `death`, `hit`, `meet`,
  `post_combat_idle`, `wounded`).

## Patrol names

Several schemes read patrol path fields such as `path_walk` and `path_look`. When an object is running under a smart
terrain, relative path names are resolved against the smart terrain name. For example, `path_walk = guard_walk` in smart
terrain `zat_b40_smart_terrain` resolves to `zat_b40_smart_terrain_guard_walk`.

Use full path names when the path does not belong to the active smart terrain.

## Before testing a section

- Supply every required field for the selected scheme.
- Keep `path_walk` and `path_look` distinct where both are used.
- Use `sr_idle` for a state that only waits for switch rules.
