# mob_combat

`mob_combat` is the generic monster combat switch scheme. It listens for monster combat events and uses common switch
conditions to move the object to another section.

## Parameters

`mob_combat` has no scheme-specific config fields. It reads common switch fields from the section.

| Field                          | Type                       | Description                                                |
| ------------------------------ | -------------------------- | ---------------------------------------------------------- |
| `on_info`, `on_info1`, ...     | condlist                   | Switch when the condlist selects another section.          |
| `on_timer`, `on_timer1`, ...   | `milliseconds \| condlist` | Switch after the section has been active for the duration. |
| `on_signal`, `on_signal1`, ... | `signal \| condlist`       | Switch when a signal is set.                               |

## Runtime behavior

The manager only acts on the combat event. If the scheme is enabled, the monster has an enemy, and the object has an
active scheme, it calls the common section switcher.

The scheme can be disabled through `SchemeMobCombat.disable`, which sets its runtime `enabled` flag to `false`.

## Example

```ini
[logic]
active = mob_home@idle
on_combat = mob_combat@combat

[mob_combat@combat]
on_info = mob_walker@attack
```

## Notes

- This is normally referenced from a monster logic section through `on_combat`.
- It does not perform combat movement by itself.
