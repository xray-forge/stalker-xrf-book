# Online and offline

X-Ray keeps two related views of many objects:

- a server-side ALife object, which can exist while the object is offline;
- a client-side game object, which exists when the object is online and active on the current level.

Online/offline state is not the same as alive/dead. An offline object can still exist in simulation. An online object
has a live `game_object`, a binder, callbacks, and client-side updates.

## Offline objects

Offline objects live in the ALife simulation. They are represented by server objects and can be queried through
simulator APIs. Smart terrains, squads, NPCs, monsters, items, and level changers all have server-side behavior in
different ways.

Use server objects for simulation state, spawn data, story ids, smart terrain membership, and logic that must survive
outside the active client bubble.

## Online objects

When an object switches online, the engine creates or activates a client-side `game_object` and attaches an
`object_binder`. XRF uses binders to register object state, set callbacks, emit events, and update active schemes.

Use online objects for:

- direct game object methods;
- visible object state;
- callbacks such as hit, death, use, inventory, or task updates;
- per-frame or throttled client-side updates.

Do not assume `level.object_by_id(id)` succeeds for an offline object. Use the simulator/server object path when the
object may be offline.

## Switching

The engine decides when objects switch online or offline based on ALife rules, level state, distance, and object type.
Console variables such as `al_switch_distance`, `al_objects_per_update`, and related ALife settings affect this process.

XRF binders receive `net_spawn(...)` when an object goes online and `net_destroy()` when it goes offline or is
destroyed. Put registration and callback setup in the online path, and cleanup in the offline path.
