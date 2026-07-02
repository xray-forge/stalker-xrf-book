# Runtime Events and Timers

`EventsManager` is the internal publish-subscribe layer for runtime lifecycle changes. It also owns game-time intervals
and timeouts through `AbstractTimersManager`.

Use events when a binder, manager, scheme, or server object needs to announce a lifecycle change without directly
depending on every listener.

## Event Dispatch

Events are declared in `EGameEvent` under `src/engine/core/managers/events/events_types.ts`.

Register callbacks through the manager:

```ts
getManager(EventsManager).registerCallback(EGameEvent.ACTOR_UPDATE, this.onActorUpdate, this);
```

Emit events through the manager or the static helper:

```ts
EventsManager.emitEvent(EGameEvent.GAME_STARTED, isNewGame);
```

Callbacks can be registered with a context. When context is provided, the callback is called with that context.

## Event Groups

`EGameEvent` covers:

- actor registration, online/offline, reinit, death, item, trade, sleep, and update ticks;
- stalker and monster registration, hit, death, and interaction;
- helicopter, squad, smart terrain, smart cover, zone, and item lifecycle;
- task, treasure, surge, notification, hit, and UI menu events;
- save/load and level-change events;
- debug dump requests.

Prefer adding a specific event over overloading an unrelated existing one. Listeners should be able to infer why they
were called from the event name.

## Timers

`EventsManager` extends `AbstractTimersManager`.

Use:

```ts
const [cancel] = EventsManager.registerGameTimeout(callback, 1000);
const [stop] = EventsManager.registerGameInterval(callback, 500);
```

Intervals assert that the period is at least `50` milliseconds. Both intervals and timeouts receive the actual elapsed
offset when they run.

Timers are processed by `ActorBinder.update()` through `eventsManager.tick()`. They advance on actor updates, not as
independent operating-system timers.

## Cleanup

Unregister callbacks in the lifecycle owner that registered them:

- managers should unregister in `destroy()`;
- binders should reset object callbacks in offline cleanup;
- one-shot timers remove themselves after running;
- long-lived intervals should keep and call the cancel function when the owner is destroyed.

High-frequency events such as `ACTOR_UPDATE` should stay light. Use throttled actor update events or an interval when
work does not need to run every actor tick.
