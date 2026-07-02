# Runtime Binders

Binders attach TypeScript lifecycle code to online xray game objects. They are registered through the `bind` extern
module in `src/engine/scripts/bind.ts`.

Use binders for client-side lifecycle glue: registering an object, installing engine callbacks, initializing logic, and
cleaning up when the object goes offline.

## Binder Families

| Family        | Binder functions                                                                     |
| ------------- | ------------------------------------------------------------------------------------ |
| Creatures     | `actor`, `stalker`, `monster`, `crow`                                                |
| Zones         | `restrictor`, `anomaly_zone`, `anomaly_field`, `camp`, `arena_zone`, `level_changer` |
| Physical      | `physic_object`, `door`, `campfire`, `artefact`, `phantom`, `signal_light`           |
| Items         | `weapon`, `helmet`, `outfit`                                                         |
| Smart systems | `smart_terrain`, `smart_cover`                                                       |
| Helicopter    | `helicopter`                                                                         |

Some binder factories are conditional:

- `arena_zone` binds only when the spawn ini contains `arena_zone`;
- `helicopter` binds only when the spawn ini contains `logic`;
- `physic_object` binds only when the object has `logic` or is an inventory box;
- `smart_terrain` binds only when the spawn ini contains `smart_terrain`.

## Common Lifecycle Methods

Most binders implement some subset of:

- `reinit`: reset local and registry state;
- `net_spawn`: object came online;
- `update`: object update tick;
- `net_destroy`: object went offline;
- `net_save_relevant`: whether binder state should be saved;
- `save`: write binder and object logic state;
- `load`: read binder and object logic state.

Use the same read/write order in `save` and `load`. When a binder persists object logic, it usually wraps the operation
in save markers and calls `saveObjectLogic` / `loadObjectLogic`.

## Actor Binder

`ActorBinder` is the global update driver for many runtime systems.

On online switch it:

- shows indicators;
- registers actor references;
- initializes actor portable store;
- emits `ACTOR_GO_ONLINE`.

On `reinit` it registers the actor again, resets portable store, installs actor callbacks, enables unlimited ALife
update for the initial spawn buffer, schedules stable ALife updates, and emits `ACTOR_REINIT`.

On update it emits `ACTOR_UPDATE`, throttled actor update events, processes `EventsManager` timers, and updates actor
simulation availability.

## Stalker Binder

`StalkerBinder` owns online stalker setup. It creates the stalker state manager and patrol manager, sets up planners,
registers the stalker in the registry, installs callbacks, initializes sound themes, initializes object logic, and sets
up post-combat idle behavior.

On offline switch it stops sounds, emits scheme offline events, applies `on_offline` overrides, stores offline state,
and unregisters the stalker.

## Restrictor Binder

`RestrictorBinder` is a compact example for zone lifecycle:

- `reinit` resets object registry state;
- `net_spawn` registers the zone and starts looped sounds;
- first `update` initializes restrictor scheme logic;
- later `update` tracks visited state, emits scheme updates, and updates sounds;
- `net_destroy` emits scheme offline behavior, stops sounds, and unregisters the zone;
- `save` and `load` persist object logic and visited state.

## Guidelines

- Keep binders focused on object lifecycle.
- Use managers for cross-object systems.
- Use registry helpers instead of mutating registry tables directly.
- Reset engine callbacks when an object goes offline.
- Check `net_save_relevant` before assuming a binder's save/load methods are used.
