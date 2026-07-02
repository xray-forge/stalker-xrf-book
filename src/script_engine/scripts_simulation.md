# Simulation

Simulation scripts coordinate ALife squads, smart terrains, actor targeting, respawn, map spots, and online/offline
movement tasks.

The core implementation is split between `SimulationManager`, server object classes, smart terrain utilities, and squad
actions.

## Source Layout

| Source                                      | Purpose                                                      |
| ------------------------------------------- | ------------------------------------------------------------ |
| `src/engine/core/managers/simulation`       | Simulation manager, config, activity rules, target selection |
| `src/engine/core/objects/squad`             | Squad server object and reach/stay actions                   |
| `src/engine/core/objects/smart_terrain`     | Smart terrain simulation target, jobs, respawn, control      |
| `src/engine/core/objects/creature/Actor.ts` | Actor as a simulation target                                 |
| `src/engine/core/database/simulation.ts`    | Simulation registration helpers                              |
| `src/engine/core/utils/alife.ts`            | ALife update batching helpers                                |
| `src/engine/core/utils/squad`               | Squad state/action helpers                                   |

## SimulationManager

`SimulationManager` is registered during startup. It listens for:

- `DUMP_LUA_DATA`;
- `ACTOR_REGISTER`;
- `ACTOR_GO_OFFLINE`.

On actor registration it initializes default simulation squads. On actor offline it removes the actor from xray ranking
when that engine callback exists. It also saves and loads the `IS_SIMULATION_INITIALIZED` flag.

## Simulation Targets

The main simulation targets are:

- actor server object;
- squads;
- smart terrains.

Targets expose methods such as:

- `getSimulationTask`;
- `isSimulationAvailable`;
- `isValidSimulationTarget`;
- `isReachedBySimulationObject`;
- `onSimulationTargetSelected`;
- `onSimulationTargetDeselected`.

## Squads

`Squad` is an online/offline group with a faction, behavior table, assigned target, current action, map spot, story
manager, and optional scripted target condlist.

When updating, a squad either:

- follows a scripted target from `target_smart`;
- helps the actor if the helper target is available;
- selects a generic simulation target by priority.

It then runs either:

- `SquadReachTargetAction`;
- `SquadStayOnTargetAction`.

## Smart Terrains

`SmartTerrain` owns:

- simulation role and simulation properties;
- max population and arrival distance;
- job creation and job assignment;
- respawn configuration;
- campfire state;
- terrain control and alarm state;
- arriving objects and assigned job descriptors.

When a terrain is selected as a target, squad members are soft-reset offline and assigned to the terrain. When NPCs
arrive, the terrain assigns jobs from its job list.

## ALife Update Rate

`setUnlimitedAlifeObjectsUpdate()` temporarily allows all ALife objects to update, which smooths initial spawn.
`setStableAlifeObjectsUpdate()` restores the configured stable update count.

`ActorBinder.reinit()` enables unlimited updates, then schedules stable updates through an `EventsManager` timeout.

## Guidelines

- Treat squads and smart terrains as server-side simulation objects.
- Do not change target selection from only one layer. Check squad actions, simulation priority utilities, terrain
  availability, and save/load state together.
- When changing simulation persistence, update both `STATE_Write` and `STATE_Read`.
