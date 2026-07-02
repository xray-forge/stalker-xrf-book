# Updates and scheduling

The engine drives updates for online binders. XRF turns those updates into higher-level events and timers so managers do
not need to poll unrelated objects directly.

## Binder updates

An online binder can receive `update(delta)` from the engine. Use it for object-local behavior:

- actor update orchestration;
- stalker and monster state managers;
- active restrictor, anomaly, smart terrain, and physic object logic;
- sound manager updates tied to a specific object id.

Call `super.update(delta)` when overriding an engine binder method unless nearby code shows a deliberate reason not to.

## Actor update events

`ActorBinder.update(delta)` is the central XRF update pump. It emits:

- `ACTOR_FIRST_UPDATE` once after start or load;
- `ACTOR_UPDATE` every actor update;
- `ACTOR_UPDATE_100`;
- `ACTOR_UPDATE_500`;
- `ACTOR_UPDATE_1000`;
- `ACTOR_UPDATE_5000`;
- `ACTOR_UPDATE_10000`.

It also ticks the XRF timer manager and refreshes actor-related simulation object availability.

Use the throttled actor events for recurring manager work. For example, weather, input, psy, debug, and UI-related
systems can subscribe to the event cadence they actually need.

## Timers

`EventsManager` extends the timer manager and supports delayed and interval callbacks. Timers tick from the actor update
path, so they require an active actor update loop.

Use timers for short delayed script work. Do not use them as durable save/load state unless the owning manager
explicitly serializes enough data to restore the behavior.

## ALife scheduling

Offline simulation is controlled by the engine and ALife settings. XRF can temporarily adjust object processing during
startup; for example, actor reinit allows a broader ALife update window and then schedules a return to the stable
configured value.

Keep online binder updates, manager event updates, and offline ALife scheduling separate. They run at different layers
and have different save/load assumptions.
