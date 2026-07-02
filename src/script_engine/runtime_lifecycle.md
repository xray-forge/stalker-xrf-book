# Runtime Lifecycle

The runtime lifecycle is the path from xray loading Lua scripts to XRF managers, binders, schemes, and server objects
owning live gameplay state.

Read this section before changing object registration, manager startup, save/load, online/offline transitions, or event
emission order.

## Startup Flow

The main game-start callback is `start.callback(isNewGame)` in `src/engine/scripts/start.ts`. It runs for both new games
and loaded games.

Startup order:

1. update cached class ids from `classIds`;
2. register the ALife simulator;
3. register rank descriptors;
4. unlock system ini overriding;
5. register managers;
6. register schemes;
7. register extensions;
8. emit `GAME_STARTED`.

Managers, schemes, and extensions should not depend on object binders already being online during this callback. Online
objects arrive later through binder calls from the engine.

## Object Flow

The usual runtime path is:

1. xray loads script entry modules such as `register`, `bind`, and `start`.
2. `start.callback` initializes shared runtime systems.
3. xray creates an online game object and calls a function from the `bind` extern module.
4. The binder attaches a TypeScript `object_binder` subclass to the game object.
5. `net_spawn` registers the object in the runtime registry and sets up callbacks.
6. `update` runs per-frame or throttled behavior.
7. `net_destroy` unregisters callbacks and registry state when the object goes offline.
8. `save` and `load` persist binder and scheme state when the object is save-relevant.

Server-side objects use related ALife callbacks such as `on_register`, `on_unregister`, `STATE_Write`, and `STATE_Read`.

## Runtime Owners

| Owner          | Source                            | Owns                                                |
| -------------- | --------------------------------- | --------------------------------------------------- |
| Entry modules  | `src/engine/scripts`              | Engine-facing extern names and startup callbacks.   |
| Binders        | `src/engine/core/binders`         | Client object lifecycle and object-local glue.      |
| Managers       | `src/engine/core/managers`        | Cross-object systems and singleton runtime state.   |
| Registry       | `src/engine/core/database`        | Shared runtime tables and focused helper APIs.      |
| Schemes        | `src/engine/core/schemes`         | Object logic sections driven by configs.            |
| Server objects | `src/engine/core/objects`         | ALife-side registration, simulation, and save data. |
| Events         | `src/engine/core/managers/events` | Internal publish-subscribe events and game timers.  |

Keep state near the owner that controls its lifecycle. A binder should not become the permanent owner of a cross-object
system. A manager should not store short-lived object state that belongs in `registry.objects`.

## First Places To Check

- For startup order, read `src/engine/scripts/start.ts`.
- For binder factories, read `src/engine/scripts/bind.ts`.
- For manager startup, read `src/engine/scripts/register/managers_registrator.ts`.
- For manager registry helpers, read `src/engine/core/database/managers.ts`.
- For shared state, read `src/engine/core/database/registry.ts`.
- For save/load coordination, read `src/engine/core/managers/save/SaveManager.ts`.

## Editing Checklist

- Identify whether the change belongs to a binder, manager, registry helper, scheme, or server object.
- Check both online and offline paths.
- Check save and load paths if state must survive reload.
- Preserve event order unless the task is specifically about event behavior.
- Add focused tests near the lifecycle owner.
