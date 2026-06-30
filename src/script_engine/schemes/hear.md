# hear

`hear` parses `on_sound` rules from the current section and switches schemes when the object hears a matching sound. It
is shared by stalker and monster binders.

## Parameters

Add one or more `on_sound` lines to the active section:

| Field                        | Type                          | Required | Description |
| ---------------------------- | ----------------------------- | -------- | ----------- |
| `on_sound`, `on_sound1`, ... | pipe-separated parameter list | no       | `story_id   | sound_type | distance | power | condlist` |

Parameter meanings:

| Position | Name         | Description                                                                                  |
| -------- | ------------ | -------------------------------------------------------------------------------------------- |
| 1        | `story_id`   | Story id of the sound source. The runtime uses `any` when the source object has no story id. |
| 2        | `sound_type` | Mapped sound type, such as `WPN_shoot`, `WPN_hit`, `ITM_drop`, or `MST_attack`.              |
| 3        | `distance`   | Maximum distance from the heard sound position to the listening object.                      |
| 4        | `power`      | Minimum heard sound power.                                                                   |
| 5        | `condlist`   | Section condlist evaluated when the sound matches.                                           |

## Runtime behavior

On reset, the scheme scans every line in the section and stores entries whose field name matches `on_sound` with an
optional numeric suffix.

On a hear callback, the scheme first forwards the event to the object's `danger` manager when present. It then resolves
the source story id, maps the engine sound mask to a sound type enum, checks configured distance and power, and
evaluates the stored condlist. A non-empty selected section switches the object to that section. An empty selected
section removes that hear rule.

## Example

```ini
[walker@guard]
path_walk = guard_walk
path_look = guard_look
on_sound = any|WPN_shoot|40|0.2|walker@alert
```

## Notes

- Supported mapped sound names are defined by the TypeScript `ESoundType` enum: weapon, item, monster, and `NIL`
  variants.
- Rules are keyed by source story id and sound type. Multiple rules for the same pair overwrite the same stored slot.
