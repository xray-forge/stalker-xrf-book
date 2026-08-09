# Gamedata CLI

Gamedata commands validate one assembled gamedata directory. Run them after building gamedata and before launching or
packaging it.

## `verify-gamedata`

```powershell
xrf-cli verify-gamedata ./target/gamedata
```

`ROOT` is the required positional path to the assembled gamedata directory. The command reads configs from
`ROOT/configs` and requires `ROOT/configs/system.ltx`.

## Options

- `-i, --ignore <names...>`: ignored files or folders. Multiple names are comma-separated.
- `--checks <checks...>`: selected verification checks. If omitted, all checks run.
- `--report <path>`: write a JSON verification report.
- `--silent`: disable logging.
- `-v, --verbose`: enable verbose logging.
- `-s, --strict`: fully validate expensive asset payloads.

Accepted check names are `animations`, `levels`, `ltx`, `meshes`, `particles`, `particles-usage`, `scripts`, `shaders`,
`sounds`, `spawns`, `textures`, `weapons`, and `weathers`. The script check parses emitted `.script` files with the
LuaJIT syntax dialect.

If `--ignore` is omitted, the command ignores common repository and unpacked-source entries: `.git`, `.idea`,
`particles_unpacked`, `textures_unpacked`, `.gitignore`, `.gitattributes`, `README.md`, and `LICENSE`.

## Checks and rules

A check is a group of related verification, selected with `--checks`. Inside a check, each individual violation is
attributed to a rule, and the rule identifier is what appears in findings and in the JSON report. Rule identifiers are
`<check>.<rule>`:

| Check             | Rules                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `animations`      | `animations.player-hud`, `animations.hud-item`, `animations.motion-collision`                                                                                                                                                                                                                                                                                                                                                                                                                              |
| `levels`          | `levels.ai-guid`, `levels.ai-node-count`, `levels.ai-version`, `levels.cform-version`, `levels.details-pair`, `levels.file-empty`, `levels.file-truncated`, `levels.graph-duplicate`, `levels.graph-guid`, `levels.header-version`, `levels.level-guid`, `levels.ltx-read`, `levels.map-texture`, `levels.missing-bundle`, `levels.missing-file`, `levels.orphan-bundle`, `levels.roster-conflict`, `levels.shader-reference`, `levels.shaders-chunk`, `levels.texture-reference`, `levels.undeclared-map` |
| `ltx`             | `ltx.formatting`, `ltx.schema`, `ltx.verification`                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| `meshes`          | `meshes.path`, `meshes.read`, `meshes.validation`, `meshes.motion-read`, `meshes.motion-validation`, `meshes.shader-library`                                                                                                                                                                                                                                                                                                                                                                               |
| `particles`       | `particles.library`, `particles.texture`                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| `particles-usage` | `particles-usage.reference`, `particles-usage.spawn`, `particles-usage.spawn-custom-data`                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `scripts`         | `scripts.path`, `scripts.read`, `scripts.syntax`                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| `shaders`         | `shaders.renderer-root`, `shaders.lua-syntax`, `shaders.source-read`, `shaders.source-invalid`, `shaders.include-missing`, `shaders.include-cycle`, `shaders.include-syntax`                                                                                                                                                                                                                                                                                                                               |
| `sounds`          | `sounds.files`, `sounds.references`                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| `spawns`          | `spawns.path`, `spawns.read`                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| `textures`        | `textures.path`, `textures.read`, `textures.dds`                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| `weapons`         | `weapons.validation`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| `weathers`        | `weathers.definitions`, `weathers.files`, `weathers.validation`                                                                                                                                                                                                                                                                                                                                                                                                                                            |

`checks.execution` is reported when a check itself fails to run, rather than when content is invalid.

The animation rules validate player HUD and item motions. Missing item motions are allowed where the engine falls back
to `idle`; duplicate motion names across banks in one HUD namespace are reported because their resolution is ambiguous.

## JSON report

Pass `--report` to write the result for CI or other tooling:

```powershell
xrf-cli verify-gamedata ./target/gamedata --checks sounds,weathers --report ./verification-report.json
```

The report is a single object with camelCase keys:

```json
{
  "checks": [
    {
      "durationMs": 114,
      "findings": [],
      "status": "passed",
      "summary": "122/122 weather files valid",
      "verificationType": "weathers"
    }
  ],
  "durationMs": 311,
  "status": "passed"
}
```

`status` is one of `passed`, `failed`, `error`, `incomplete`, or `skipped`. The top-level status is the most severe
individual status. `incomplete` means a check could cover only part of its expected input; `durationMs` is `null` when a
check did not run.

Each entry of `findings` describes one violation:

| Field       | Meaning                                  |
| ----------- | ---------------------------------------- |
| `ruleId`    | Rule identifier from the table above.    |
| `assetPath` | Root-relative asset path. May be absent. |
| `message`   | Human-readable description.              |

Findings are ordered by asset path, rule, then message, so two reports over the same gamedata can be compared directly.

## Examples

```powershell
xrf-cli verify-gamedata ./target/gamedata
xrf-cli verify-gamedata ./target/gamedata --checks scripts,ltx
xrf-cli verify-gamedata ./target/gamedata --checks weathers
xrf-cli verify-gamedata ./target/gamedata --checks sounds --strict
xrf-cli verify-gamedata ./target/gamedata --report ./verification-report.json
xrf-cli verify-gamedata ./target/gamedata --ignore .git,textures_unpacked --strict
```

## Result

The command exits with a non-zero status unless the overall result is `passed`, including when verification is skipped
or incomplete. In normal logging mode it prints each failure message before exiting.

The command validates the files present in the assembled tree, including generated scripts and configs. It does not
validate source repositories or files that were not included in the build.
