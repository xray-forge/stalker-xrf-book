# Known xray engine forks

XRF targets the Call of Pripyat style X-Ray/OpenXRay API used by the local engine and type declarations. Forks are
useful reference points, but they are not a compatibility guarantee. Verify bindings, console commands, save/load
behavior, and script callbacks against the exact executable you ship.

## Common references

| Fork or baseline                                          | Notes                                                                                                |
| --------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| Original Call of Pripyat engine and gamedata              | Canonical behavior reference for vanilla script and resource behavior.                               |
| [OpenXRay / xray-16](https://github.com/OpenXRay/xray-16) | Open-source X-Ray continuation used as the main local engine reference for XRF.                      |
| Call of Chernobyl engine family                           | Useful second opinion for evolved CoP-era behavior, but not the canonical baseline.                  |
| Anomaly / X-Ray Monolith family                           | Heavily modified fork family. Expect changed exports, fixes, callbacks, and engine-side assumptions. |
| [OGSR Engine](https://github.com/OGSR/OGSR-Engine)        | Shadow of Chernobyl oriented fork with different compatibility expectations.                         |
| Oxygen and other experimental forks                       | Treat as fork-specific until the script API is checked directly.                                     |

## Compatibility checklist

Before moving XRF scripts to a fork, check:

- luabind class names and exported functions;
- `object_binder` method behavior;
- game class identifiers and section-to-class mappings;
- console command names and accepted value types;
- command line flags used by your launcher;
- save/load packet order and marker expectations;
- availability of Lua libraries such as `jit`, `ffi`, `marshal`, and `lfs`;
- callback names and callback argument order;
- ALife online/offline switching behavior.

When a fork disagrees with vanilla resources and OpenXRay source, document the fork behavior as fork-specific instead of
treating it as the default.
