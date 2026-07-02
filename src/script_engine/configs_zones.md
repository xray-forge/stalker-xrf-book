# Zones and anomalies

Zone configs describe anomaly fields, anomaly mines, campfires, teleport and no-gravity zones, space restrictors, level
changers, and composite anomaly zones that can spawn artifacts. Static zone sections live under
`src/engine/configs/zones`; script-level zone objects live in `src/engine/configs/objects/zone_objects.ltx`.

Use this page when you need to decide whether a behavior belongs in a base zone section, a script object section, or a
scenario-specific anomaly config.

## Source files

| Source                                                     | Purpose                                                                                                                       |
| ---------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| `zones/index.ltx`                                          | Includes all zone config files into the final game config tree.                                                               |
| `zones/zone_base.ltx`                                      | Defines shared zone defaults. XRF sets the base zone script binding to `bind.anomaly_field`.                                  |
| `zones/zone_field_*.ltx`                                   | Defines acidic, psychic, radioactive, and thermal field zones.                                                                |
| `zones/zone_mine_*.ltx` and `zones/zone_minefield.ltx`     | Defines electric, gravitational, acidic, thermal, and minefield anomaly variants.                                             |
| `zones/zone_campfire.ltx`                                  | Defines campfire zone sections.                                                                                               |
| `zones/zone_teleport.ltx` and `zones/zone_nogravity.ltx`   | Defines special movement or transition zones.                                                                                 |
| `zones/zone_burningfuzz.ltx` and `zones/zone_fireball.ltx` | Defines additional anomaly families.                                                                                          |
| `objects/zone_objects.ltx`                                 | Defines script-level zone objects such as `space_restrictor`, `anomal_zone`, `camp_zone`, `level_changer`, and `script_zone`. |
| `$scheme/zone.scheme.ltx`                                  | Validates zone fields and base zone fields.                                                                                   |
| `scripts/**/anomaly/*.ltx`                                 | Defines scenario-specific composite anomaly configs read by `AnomalyZoneBinder`.                                              |

## Runtime binding

Zone-related sections attach runtime behavior through `script_binding`.

| Binding              | Used by                                                                                            |
| -------------------- | -------------------------------------------------------------------------------------------------- |
| `bind.anomaly_field` | Regular anomaly field or mine sections based on `zone_base`.                                       |
| `bind.anomaly_zone`  | Composite `anomal_zone` script objects that coordinate fields, artifact layers, and respawn rules. |
| `bind.restrictor`    | `space_restrictor` sections.                                                                       |
| `bind.camp`          | `camp_zone` sections.                                                                              |
| `bind.campfire`      | Campfire zone sections.                                                                            |
| `bind.level_changer` | Level changer sections.                                                                            |
| `bind.arena_zone`    | Arena script zones when the section is present.                                                    |

The binding tells XRF which TypeScript binder should manage the object. The engine class still controls the underlying
X-Ray object type, collision behavior, and native zone behavior.

## Base zone sections

Most static anomaly sections inherit from `[zone_base]` or a specialized zone parent and use `$scheme = $zone`.

Common zone fields include:

- particle and sound names for idle, entrance, hit, and blowout states;
- hit settings such as hit type, hit impulse, and power values;
- light, wind, postprocess, and visual shape settings;
- artifact handling flags such as `ignore_artefacts`;
- spawn metadata and editor fields.

Use inheritance for variants such as weak, average, and strong anomaly sections. Keep the shared behavior on the parent
section and override only the values that differ.

## Script zone objects

`objects/zone_objects.ltx` defines script-level objects that are not ordinary anomaly damage sections.

Important sections include:

- `[space_restrictor]`, managed by `bind.restrictor`;
- `[anomal_zone]`, managed by `bind.anomaly_zone`;
- `[camp_zone]`, managed by `bind.camp`;
- `[level_changer]`, managed by `bind.level_changer`;
- `[script_zone]`, used for arena-style script zones.

Choose these sections when the object is meant to drive script logic, travel, restrictions, camps, or composite anomaly
behavior rather than a standalone damage field.

## Composite anomaly zones

Composite anomaly zones are managed by `AnomalyZoneBinder`. The binder reads the spawned object's `anomal_zone` section
and may also read an additional config file from the `cfg` field.

The binder reads fields such as:

- `layers_count`;
- `respawn_tries`;
- `max_artefacts`;
- `applying_force_xz` and `applying_force_y`;
- `artefacts`;
- `start_artefact`;
- `artefact_ways`;
- `field_name`;
- `coeff`;
- `coeffs_section`.

Each `layer_N` section can override artifact counts, respawn tries, maximum artifacts, forces, artifact lists, starting
artifacts, and waypoint lists for that layer.

Use composite anomaly configs when the gameplay object needs to coordinate several anomaly fields and artifact spawning
rules. Use a normal zone section when you only need an individual field, mine, or damage source.

## Editing checklist

When changing zones or anomalies:

- keep `script_binding` aligned with the section's runtime role;
- do not mix regular field sections with `anomal_zone` composite configs;
- keep `$scheme = $zone` on sections validated by `zone.scheme.ltx`;
- verify referenced particles, sounds, postprocess names, artifact sections, and waypoint names;
- check story links and map placement when changing level changers or restrictors;
- run LTX validation after config or schema changes:

```powershell
npm run cli verify ltx
```
