# sr_no_weapon

`sr_no_weapon` tracks whether the actor is inside a restrictor where weapons should be disabled. It emits enter and
leave events and records the zone in `registry.noWeaponZones`.

Use it for safe areas, story spaces, and bases where the actor should not be able to keep a weapon raised.

## Parameters

`sr_no_weapon` has no scheme-specific fields. It reads common switch fields from the section.

| Field                        | Type                       | Description                                                |
| ---------------------------- | -------------------------- | ---------------------------------------------------------- |
| `on_info`, `on_info1`, ...   | condlist                   | Switch when the condlist selects another section.          |
| `on_timer`, `on_timer1`, ... | `milliseconds \| condlist` | Switch after the section has been active for the duration. |
| `on_actor_inside`            | condlist                   | Switch while the actor is inside the current restrictor.   |
| `on_actor_outside`           | condlist                   | Switch while the actor is outside the current restrictor.  |

## Runtime behavior

On activation, the manager removes the zone's previous registry entry, resets its local actor state, and immediately
checks whether the actor is inside the restrictor.

When the actor enters the zone, it:

- sets `registry.noWeaponZones[zone_id] = true`;
- emits `ACTOR_ENTER_NO_WEAPON_ZONE`.

When the actor leaves the zone, it:

- sets `registry.noWeaponZones[zone_id] = false`;
- emits `ACTOR_LEAVE_NO_WEAPON_ZONE`.

If a section switch happens while the actor is inside, the manager emits the leave path before switching away.

## Example

```ini
[logic]
active = sr_no_weapon@base

[sr_no_weapon@base]
on_info = {+base_alarm} sr_idle@disabled
```

## Notes

- The actual weapon hiding/UI behavior is handled by systems listening to the registry/events, not by this manager.
- Use `sr_idle` or another section when the no-weapon zone should be disabled.
