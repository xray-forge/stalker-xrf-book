# Server Objects

Server object classes extend xray `cse_alife_*` classes and run on the ALife side of the engine. They register story
links, simulation targets, save/load data, and object-specific ALife behavior.

Runtime server classes live under `src/engine/core/objects`.

## Source Layout

| Source                  | Purpose                                                                       |
| ----------------------- | ----------------------------------------------------------------------------- |
| `objects/creature`      | Actor, stalker, and monster server classes                                    |
| `objects/item`          | Item, weapon, ammo, outfit, helmet, detector, torch, box, and related classes |
| `objects/physic`        | Scripted physical server object classes                                       |
| `objects/zone`          | Restrictor, anomaly, torrid, and visual zone classes                          |
| `objects/squad`         | Online/offline squad group class and actions                                  |
| `objects/smart_terrain` | Smart terrain class, jobs, respawn, and control                               |
| `objects/smart_cover`   | Smart cover server representation                                             |
| `objects/helicopter`    | Helicopter server class                                                       |
| `objects/level`         | Level changer server class                                                    |

## Class Registration

Server classes are linked to engine class ids through the registration flow exposed by `register.registerGameClasses`.
Class-id helpers are implemented in `src/engine/scripts/register`.

The class-id helpers distinguish game class ids, UI class ids, and object class ids. Unknown game types abort during
class-id resolution.

## Common Lifecycle

Server object classes usually implement some subset of:

- `on_register`;
- `on_unregister`;
- `STATE_Write`;
- `STATE_Read`;
- `on_death`;
- `update`;
- engine-specific task methods.

Registration usually updates `registry`, story links, simulation objects, and events. Unregistration must reverse those
links.

## Actor

The actor server object registers actor server state, story id `actor`, and simulation participation. It delegates
server save/load to `SaveManager` and emits `ACTOR_REGISTER`, `ACTOR_UNREGISTER`, and `ACTOR_DEATH`.

As a simulation target, the actor can be selected only when actor simulation is allowed and safe-zone restrictions do
not exclude it.

## Squad

`Squad` extends `cse_alife_online_offline_group`. It is both a server group and a simulation target.

It owns:

- squad target condlists;
- faction behavior;
- current simulation action;
- assigned terrain and target ids;
- map spot state;
- member registration;
- scripted target rotation;
- save/load for target, respawn, and terrain assignment state.

Squads select either `SquadReachTargetAction` or `SquadStayOnTargetAction` depending on whether the current target is
already reached.

## Smart Terrain

`SmartTerrain` extends `cse_alife_smart_zone`. It owns terrain simulation, job assignment, respawn configuration,
campfires, map spot state, alarm/control state, arriving objects, and job save/load data.

Smart terrain registration creates jobs and simulation descriptors. NPC registration either assigns a job immediately or
marks the object as arriving until it reaches the terrain.

## Guidelines

- Server object state must be saved and loaded in matching order.
- Register story links and simulation objects on `on_register`; unregister them on `on_unregister`.
- Keep client-side behavior in binders or schemes. Server classes should own ALife state and server persistence.
