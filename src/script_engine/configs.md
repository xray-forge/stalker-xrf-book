# Configs

XRF stores game configuration source under `src/engine/configs`. The build copies static `.ltx` and `.xml` files and
generates additional `.ltx` or `.xml` files from TypeScript sources.

Config files are runtime behavior. Script configs can switch schemes, call effects, check conditions, spawn objects,
define smart terrain jobs, describe tasks, load dialogs, configure weapons, and drive weather.

## Domain pages

The config chapter is split by the kind of runtime data being edited.

| Page                                                          | Use it for                                                                        |
| ------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| [Scheme](configs_scheme.md)                                   | How `$scheme/*.scheme.ltx` files validate LTX sections.                           |
| [Condlists](configs_condlists.md)                             | Conditional expressions used by script configs and scheme switching.              |
| [Dialogs](configs_dialogs.md)                                 | Dialog XML sources, phrase graphs, and dialog-related config data.                |
| [Scripts](configs_scripts.md)                                 | Story script configs, scheme sections, effects, conditions, and logic activation. |
| [Creatures](configs_creatures.md)                             | Actor, stalker, monster, crow, and online/offline group configs.                  |
| [Zones and anomalies](configs_zones.md)                       | Anomaly fields, anomaly zones, restrictors, camp zones, and level changers.       |
| [Smart terrains and jobs](configs_smart_terrains_and_jobs.md) | Smart terrain population, jobs, respawn, and simulation behavior.                 |
| [Treasures](configs_treasures.md)                             | Treasure descriptors and hidden stash behavior.                                   |
| [Weapons](configs_weapons.md)                                 | Weapon section layout and generated weapon config helpers.                        |
| [Weather](configs_weather.md)                                 | Weather cycles, graph sources, and weather config generation.                     |

## Source types

| Source type            | Build behavior                                                        |
| ---------------------- | --------------------------------------------------------------------- |
| `.ltx`                 | Copied to `target/gamedata/configs`.                                  |
| `.xml`                 | Copied to `target/gamedata/configs`.                                  |
| `.ts`                  | Imported by the CLI and rendered to `.ltx` with `renderJsonToLtx`.    |
| `.tsx`                 | Imported by the CLI and rendered to `.xml` with `renderJsxToXmlText`. |
| `$scheme/*.scheme.ltx` | Used by `verify-ltx` to validate config shape.                        |

Dynamic `.ts` LTX sources must export `create()` or `config`. Dynamic `.tsx` XML sources must export `create()`.

## Includes and inheritance

LTX files use normal X-Ray include and inheritance syntax:

```ltx
#include "items\weapons\base.ltx"

[wpn_example]:identity_immunities
class = WP_AK74
```

Generated LTX sources can express includes and section inheritance through the helper symbols used by `renderJsonToLtx`.

## Validation

Run LTX validation after changing config shape, includes, inheritance, or `$scheme` coverage:

```powershell
npm run cli verify ltx
```

Use strict validation when the change is specifically about schema coverage:

```powershell
npm run cli verify ltx -- --strict
```

For generated configs, also run a focused build:

```powershell
npm run cli build -- --filter configs
```

Do not edit generated files under `target/`. Fix the source config or generator instead.
