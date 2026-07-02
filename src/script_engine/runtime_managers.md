# Runtime Managers

Managers are singleton runtime services stored in the registry. They own cross-object systems such as events, save/load,
sound, simulation, trade, tasks, weather, upgrades, UI state, and debugging.

Managers extend `AbstractManager` from `src/engine/core/managers/abstract/AbstractManager.ts`.

## Manager Access

Use the registry helper that matches the lifecycle you need:

```ts
getManager(SoundManager);
getWeakManager(SoundManager);
getManagerByName("SoundManager");
```

`getManager(ManagerClass)` is the normal path. It returns the existing singleton or initializes one.

`getWeakManager(ManagerClass)` returns `null` if the manager is not initialized.

`getManagerByName(name)` is mainly for circular-reference cases where the class reference is not available. It cannot
initialize a missing manager.

## Startup Managers

`registerManagers()` initializes the startup manager list during `start.callback`.

The current startup list is:

- `ActorInputManager`;
- `ActorInventoryMenuManager`;
- `DatabaseManager`;
- `DebugManager`;
- `DialogManager`;
- `EventsManager`;
- `GameSettingsManager`;
- `LoadScreenManager`;
- `LoadoutManager`;
- `MapDisplayManager`;
- `MusicManager`;
- `NotificationManager`;
- `PdaManager`;
- `PhantomManager`;
- `ProfilingManager`;
- `ReleaseBodyManager`;
- `SaveManager`;
- `SimulationManager`;
- `SleepManager`;
- `SoundManager`;
- `StatisticsManager`;
- `TaskManager`;
- `TradeManager`;
- `TravelManager`;
- `TreasureManager`;
- `UpgradesManager`;
- `WeatherManager`.

Other managers can still be initialized lazily with `getManager`. For example, `SaveManager` initializes `SurgeManager`
after `ACTOR_REINIT`.

## Lifecycle Methods

`AbstractManager` defines:

- `initialize()`;
- `destroy()`;
- `update(delta)`;
- `save(packet)`;
- `load(reader)`.

The base `update`, `save`, and `load` methods abort. Implement only the methods the manager actually supports.

`disposeManager` calls `destroy()`, marks the manager as destroyed, and removes it from both registry maps.

## Common Patterns

Managers that listen to events usually subscribe in `initialize()` and unsubscribe in `destroy()`.

Managers with persistent state write to net packets through `SaveManager` or to dynamic save data through helpers. Keep
save and load order synchronized.

Managers with delayed work should check `isDestroyed` before doing work after disposal.

## Where To Add Behavior

- Use a manager for shared behavior across many objects.
- Use a binder when the behavior belongs to one online object.
- Use a scheme manager when the behavior belongs to one config scheme.
- Use a database helper when the behavior is narrow registry access.

Do not construct managers directly in runtime code. Use `getManager` unless a test is intentionally isolating a manager
class.
