# Object Binders

Object binders attach TypeScript lifecycle code to online xray game objects. They are client-side wrappers around
`object_binder` and are registered through the `bind` extern module.

## Binder Registration

`src/engine/scripts/bind.ts` exposes one function per bindable object category:

- creatures: `actor`, `stalker`, `monster`, `crow`;
- zones: `restrictor`, `anomaly_zone`, `anomaly_field`, `camp`, `arena_zone`, `level_changer`;
- physical objects: `physic_object`, `door`, `campfire`, `artefact`, `phantom`, `signal_light`;
- items: `weapon`, `helmet`, `outfit`;
- simulation objects: `smart_terrain`, `smart_cover`;
- helicopter: `helicopter`.

Some binders are conditional. For example, `physic_object` binds only when the object has a `[logic]` section or is an
inventory box, and `smart_terrain` binds only when the spawn ini contains `[smart_terrain]`.

## Lifecycle Methods

Most binders implement some subset of:

- `reinit`: reset local state and registry state;
- `net_spawn`: object came online;
- `update`: per-frame object update;
- `net_destroy`: object went offline;
- `save` and `load`: client-side save state;
- `net_save_relevant`: whether binder state should be saved.

Server object classes use related server callbacks such as `on_register`, `on_unregister`, `STATE_Write`, and
`STATE_Read`.

## ActorBinder

`ActorBinder` registers the actor object, initializes portable store, emits actor lifecycle events, and drives global
update ticks.

On each update it emits:

- `ACTOR_UPDATE`;
- `ACTOR_UPDATE_100`;
- `ACTOR_UPDATE_500`;
- `ACTOR_UPDATE_1000`;
- `ACTOR_UPDATE_5000`;
- `ACTOR_UPDATE_10000`.

It also ticks `EventsManager` timers and updates simulation object availability for the actor server object.

## StalkerBinder

`StalkerBinder` owns online stalker runtime setup:

- resets object registry state;
- creates the state manager and patrol manager;
- sets up state and motivation planners;
- registers the stalker in the global registry;
- initializes sound themes, reach-task behavior, object logic, post-combat idle, trade, and light behavior;
- forwards hit, death, use, sound, and patrol events to schemes and managers.

On offline switch it emits scheme events, runs `on_offline` overrides, stores offline state, stops sounds, and
unregisters the stalker.

## RestrictorBinder

`RestrictorBinder` registers zones, initializes restrictor scheme logic on first update, tracks visited restrictors,
emits visit events, updates active schemes, and persists visited state.

## Guidelines

- Keep binders focused on lifecycle glue.
- Put reusable behavior in schemes, managers, or utilities.
- Always unregister callbacks and registry entries when an object goes offline.
- When adding save/load fields, update the matching load path in the same order.
