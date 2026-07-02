# Lifecycle

The engine lifecycle is split between global startup, server-side ALife objects, client-side game objects, XRF binders,
and XRF managers. Most script bugs come from mixing those scopes.

## Global startup

Global startup happens once per game process and again at specific script reload points depending on the engine. In XRF,
the important global scripts are:

- `_g.script`, which preloads core entry points and registers externals;
- `register.script`, which registers classes and callback globals;
- `start.script`, which initializes managers, schemes, extensions, and emits `GAME_STARTED`;
- `bind.script`, which returns binder classes for online objects.

Do not put per-save or per-object state in module globals unless it is intentionally reset during the relevant lifecycle
step.

## Binder lifecycle

Client-side game objects use engine `object_binder` lifecycle methods. XRF binders override the methods they need:

| Method                    | When it is used                                                                                                                       |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| `reinit()`                | Reinitializes binder state and callbacks. Actor reinit also resets portable store state and schedules ALife update stabilization.     |
| `net_spawn(serverObject)` | Called when the object goes online and receives its server object. Return `false` to reject spawn after `super.net_spawn(...)` fails. |
| `update(delta)`           | Called while the client object is online. Use it for object-local work, not broad global polling.                                     |
| `net_destroy()`           | Called when the object goes offline or is destroyed. Remove callbacks and unregister object state here.                               |
| `save(packet)`            | Write client-side state to the save packet. Preserve marker and write order.                                                          |
| `load(reader)`            | Read client-side state from the save packet in the same order it was written.                                                         |

Actor, stalker, monster, restrictor, smart terrain, physic object, and item binders all follow this pattern.

## Manager lifecycle

Managers extend `AbstractManager`. A manager can implement:

- `initialize()` to register callbacks or allocate state;
- `destroy()` to unregister callbacks and mark state as disposed;
- `update(delta)` when it is driven by an update event;
- `save(packet)` and `load(reader)` when it owns serialized state.

Managers should subscribe through `EventsManager` rather than being called from unrelated binders. This keeps object
lifecycle code small and makes save/load ownership clearer.

## Event lifecycle

`EventsManager` owns typed event subscriptions. Binders and game objects emit events such as:

- `ACTOR_GO_ONLINE`, `ACTOR_GO_OFFLINE`, `ACTOR_REINIT`;
- `ACTOR_UPDATE`, `ACTOR_UPDATE_100`, `ACTOR_UPDATE_500`, `ACTOR_UPDATE_1000`, `ACTOR_UPDATE_5000`,
  `ACTOR_UPDATE_10000`;
- `STALKER_DEATH`, `MONSTER_DEATH`, `HIT`;
- `GAME_SAVE`, `GAME_SAVED`, `GAME_LOAD`, `GAME_LOADED`;
- `BEFORE_LEVEL_CHANGE` and `GAME_STARTED`.

Actor update drives the global timer manager tick and the throttled actor update events. Prefer those throttled events
for recurring manager work that does not need every frame.

## Save and load lifecycle

Save/load code must preserve packet order. XRF commonly wraps sections with save/load markers, calls the superclass
method, then writes or reads owned state.

If a manager or binder adds state to a save packet, update the corresponding load code in the same change. Never insert
a new read without matching old saves or version handling.
