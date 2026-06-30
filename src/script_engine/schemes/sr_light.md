# sr_light

`sr_light` registers a restrictor as a light-control zone for stalkers. Other systems can query active light zones to
decide whether a stalker's torch should be on or off while the stalker is inside the zone.

## Parameters

### `light_on`

Type: boolean. Optional. Default: `false`.

Light flag returned when a stalker is inside the active zone.

The section also supports common switch fields such as `on_info`, `on_timer`, and actor-zone checks.

## Runtime behavior

On activation, the manager registers itself in `registry.lightZones`. On update, it checks common switch conditions. If
a switch happens, the manager marks itself inactive and removes the zone from the registry. Otherwise it remains active.

The manager's `checkStalker` helper returns two booleans:

- the configured `light_on` value;
- whether the checked stalker is inside this active restrictor zone.

## Example

```ini
[logic]
active = sr_light@underground

[sr_light@underground]
light_on = true
on_info = {+lab_power_restored} sr_light@off

[sr_light@off]
light_on = false
```

## Notes

- The scheme reset clears all registered light zones.
- Deactivation does not currently unregister the zone directly; updates and reset handle registry cleanup.
