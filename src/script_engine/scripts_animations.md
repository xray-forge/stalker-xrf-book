# Animations

Animation scripts define named stalker states, animation sequences, smart-cover descriptors, and helper functions used
by schemes such as `walker`, `remark`, `animpoint`, `camper`, `corpse_detection`, and `help_wounded`.

## Source Layout

| Source                                   | Purpose                                                                           |
| ---------------------------------------- | --------------------------------------------------------------------------------- |
| `src/engine/core/animation/types`        | State, animation, and patrol descriptor types                                     |
| `src/engine/core/animation/states`       | Stalker state descriptors such as movement, mental state, weapon mode, body state |
| `src/engine/core/animation/animations`   | Animation sequence tables and callbacks                                           |
| `src/engine/core/animation/animstates`   | Additional animation-state mappings                                               |
| `src/engine/core/animation/smart_covers` | Smart-cover and loophole animation descriptors                                    |
| `src/engine/core/animation/predicates`   | Predicate lists used to select animpoint-compatible animations                    |
| `src/engine/core/utils/animation.ts`     | Helpers for sequence construction                                                 |

## State Descriptors

State descriptors map a script state name to engine animation inputs. The base table defines states such as `walk`,
`run`, `patrol`, `assault`, `threat`, `hide`, `search_corpse`, `help_wounded`, and wounded states.

A descriptor can set:

- weapon animation mode, such as strapped, unstrapped, fire, drop, or none;
- movement mode, such as stand, walk, or run;
- mental state, such as free, danger, or panic;
- body state, such as standing or crouch;
- animation state or concrete animation name;
- sight direction behavior;
- force flags used by state managers.

Schemes usually pass a state name to `setStalkerState`. The state manager resolves that name through these tables and
applies the engine-level settings.

## Animation Sequences

Animation descriptors define `into`, `idle`, `rnd`, and `out` sequences. The `createSequence(...)` helper stores the
sequence as a Lua table using zero-based indexes, which matches the runtime animation code.

Sequence entries can be:

- animation names;
- arrays of candidate animation names;
- action descriptors, such as attaching or detaching an item;
- function callbacks, such as finishing corpse looting or wounded help.

Examples in the base animation table include:

- `play_guitar` and `play_harmonica`, which attach camp instruments;
- `punch`, which calls the abuse punch helper and then clears abuse state;
- `search_corpse`, which calls corpse-loot finalization;
- `help_wounded_with_medkit`, which attaches the scripted medkit and finishes wounded help.

## Smart Cover Animations

Smart-cover animation files define cover descriptions and loopholes consumed by `smartcover` and `animpoint`. The
registered `smart_covers.descriptions` extern exposes the list to xray.

Animpoint schemes can also use predicate lists to choose compatible animations from a smart-cover description when
`avail_animations` is not explicitly set.

## Common Pitfalls

- State names are runtime API. Changing a name can break LTX sections that refer to it.
- Some sequence callbacks have gameplay side effects, such as transferring loot or healing wounded NPCs.
- Smart-cover descriptors are used both by scripts and engine cover logic. Keep cover and loophole names stable.
