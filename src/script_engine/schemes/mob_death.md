# mob_death

`mob_death` handles monster death callbacks. It records the killer id and then evaluates common switch logic.

## Parameters

`mob_death` has no scheme-specific config fields. It reads common switch fields from the section.

### `on_info`, `on_info1`, ...

Type: condlist.

Switch when the condlist selects another section.

### `on_signal`, `on_signal1`, ...

Type: `signal | condlist`.

Switch when a signal is set.

### `on_timer`, `on_timer1`, ...

Type: `milliseconds | condlist`.

Switch after the section has been active for the duration.

## Runtime behavior

On death, the manager stores the killer id in the object's death state:

- `killer.id()` when a killer exists;
- `-1` when the killer is nil.

After that, it calls the common switcher for the `mob_death` state.

## Example

```ini
[logic]
active = mob_home@idle
on_death = mob_death@dead

[mob_death@dead]
on_info = nil %+bloodsucker_dead%
```

## Notes

- This is normally referenced from a monster logic section through `on_death`.
- Use condlist effects to set info portions, spawn rewards, or advance quest state.
