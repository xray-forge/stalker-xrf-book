# Configs

XRF stores game configuration source under `src/engine/configs`. The build copies static `.ltx` and `.xml` files and
generates additional `.ltx` or `.xml` files from TypeScript sources.

Config files are runtime behavior. Script configs can switch schemes, call effects, check conditions, spawn objects,
define smart terrain jobs, describe tasks, load dialogs, configure weapons, and drive weather.

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
