# sr_silence

`sr_silence` marks a restrictor as a silence zone by registering it in `registry.silenceZones`. It is used by other
systems to suppress dynamic music, usually in safe places.

## Parameters

`sr_silence` has no scheme-specific fields. It reads common switch fields from the section.

### `on_info`, `on_info1`, ...

Type: condlist.

Switch when the condlist selects another section.

### `on_timer`, `on_timer1`, ...

Type: `milliseconds | condlist`.

Switch after the section has been active for the duration.

## Runtime behavior

On activation, the scheme stores the restrictor id and name in `registry.silenceZones`.

The manager itself is empty in the current implementation. The source notes that deactivation behavior may be missing.
Use another section to control logic flow, but do not rely on this manager to unregister itself.

## Example

```ini
[logic]
active = sr_silence@base

[sr_silence@base]
on_info = {+base_alarm} sr_idle@disabled
```

## Notes

- This scheme does not currently implement update or deactivation behavior.
- Music suppression is handled by systems that read `registry.silenceZones`.
