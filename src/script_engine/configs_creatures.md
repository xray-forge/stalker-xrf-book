# Creatures

Creature configs describe the actor, stalkers, monsters, crows, and online/offline squads. They live under
`src/engine/configs/creatures` and are included through `src/engine/configs/creatures/index.ltx`.

These files are not only stats tables. A creature section also selects the engine class, script binder, physical
settings, perception values, sounds, movement tuning, immunities, and condition sections used by runtime logic.

## Source files

| Source                                 | Purpose                                                                                                                                         |
| -------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| `creatures/index.ltx`                  | Includes all creature config files into the final game config tree.                                                                             |
| `creatures/base.ltx`                   | Defines shared monster defaults and common physical death-friction parameters.                                                                  |
| `creatures/actor.ltx`                  | Defines the playable actor section, condition sections, HUD link, hit sounds, movement, and immunities.                                         |
| `creatures/m_stalker*.ltx`             | Defines stalker, monolith stalker, and zombied stalker creature sections.                                                                       |
| `creatures/m_*.ltx`                    | Defines monster species such as bloodsucker, boar, burer, chimera, controller, dog, flesh, gigant, poltergeist, pseudodog, snork, and tushkano. |
| `$scheme/creatures.stalker.scheme.ltx` | Validates actor and stalker config fields.                                                                                                      |
| `$scheme/creatures.monster.scheme.ltx` | Validates monster config fields.                                                                                                                |
| `$scheme/creatures.misc.scheme.ltx`    | Validates online/offline group sections used for squad descriptors.                                                                             |

## Runtime binding

Creature sections attach TypeScript runtime code through `script_binding`.

| Binding        | Runtime role                                                                                                    |
| -------------- | --------------------------------------------------------------------------------------------------------------- |
| `bind.actor`   | Attaches actor lifecycle, callbacks, managers, actor state, and save/load behavior.                             |
| `bind.stalker` | Attaches NPC stalker lifecycle, logic activation, callbacks, smart terrain participation, and planner behavior. |
| `bind.monster` | Attaches monster lifecycle, monster scheme activation, smart terrain participation, and offline handling.       |
| `bind.crow`    | Attaches crow-specific runtime behavior.                                                                        |

The engine class still matters. For example, actor sections use an actor class, stalker sections use stalker classes,
and monster sections use species-specific monster classes. The script binder extends that engine object with XRF runtime
logic.

## Actor sections

The actor config starts at `[actor]` and uses `$scheme = $actor`. It links the runtime binder through
`script_binding = bind.actor`.

Actor-related sections cover:

- movement and inventory limits, such as sprint, crouch, jump, and carried mass values;
- physical settings, collision damage, bones, and corpse handling;
- hit probabilities, immunities, condition sections, and hit sounds;
- actor HUD linkage through `player_hud_section`;
- default quick item slots.

Use the actor schema when adding new actor fields. If validation rejects a real engine field, update the schema instead
of bypassing validation in the config.

## Stalker sections

Stalker configs use `$scheme = $stalker` and describe NPC-level behavior visible to both the X-Ray engine and XRF
runtime.

Common stalker fields include:

- `script_binding`, usually `bind.stalker`;
- `community`, rank, terrain, and movement sections;
- visual and sound configuration;
- perception settings such as view distance, visibility, and sound perception;
- condition, damage, immunities, and material sections;
- spawn metadata and editor fields.

Scenario logic is usually not placed directly in the creature section. Stalker behavior for a specific story object is
normally driven by script configs, smart terrain jobs, and schemes.

## Monster sections

Monster configs inherit from shared monster bases and then specialize by species. The base sections provide common
movement, perception, damage, sound, and physical behavior. Species files tune attacks, locomotion, particles, sounds,
protections, and spawn sections for each monster family.

Monster runtime behavior is attached through `bind.monster`. During spawn, the binder registers the object, initializes
scheme logic with the monster scheme type, and later handles section switching, online/offline transitions, sound
cleanup, and smart terrain participation.

When editing monster configs, compare the closest existing species file first. Many fields are species-specific engine
parameters, and nearby sections are usually the best source for valid value shape.

## Online/offline groups

`$scheme/creatures.misc.scheme.ltx` validates `online_offline_group` sections. These sections describe squad-style spawn
groups and can include fields such as faction, NPC section lists, random NPC pools, target smart terrain, behavior,
death conditions, and invulnerability.

Use these sections when the config needs to describe a squad or group that moves through simulation, rather than a
single creature section.

## Editing checklist

When changing creature configs:

- keep the `$scheme` marker on sections that validation should check;
- keep `script_binding` aligned with the engine class and intended runtime binder;
- update referenced condition, immunities, sound, terrain, movement, and damage sections together;
- check script configs and smart terrain jobs before assuming behavior belongs in the creature base section;
- run LTX validation after schema or config changes:

```powershell
npm run cli verify ltx
```
