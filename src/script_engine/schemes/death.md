# death

`death` executes configured condlists when a stalker dies and stores the killer id in the death scheme state.

## Parameters

`death` reads its configuration indirectly:

### `on_death`

Location: active logic section. Type: section name. Optional.

Names the death configuration section.

### `on_info`

Location: death configuration section. Type: condlist. Optional.

First condlist executed on death.

### `on_info2`

Location: death configuration section. Type: condlist. Optional.

Second condlist executed on death.

## Runtime behavior

On reset, the scheme reads `on_death` from the current logic section. If it is set, the named section must exist. The
scheme then parses `on_info` and `on_info2` from that section.

On death, the manager stores the killer object id, or `-1` when no killer is provided. It then evaluates both parsed
condlists with the actor and the dead object as context. The return value is not used for section switching; use effects
inside the condlist for death side effects.

## Runtime sequence

1. `SchemeDeath.activate` creates the state for the stalker.
2. `SchemeDeath.add` subscribes `DeathManager`.
3. `SchemeDeath.reset` reads `on_death` from the active logic section.
4. If a death section is named, `on_info` and `on_info2` are parsed from that section.
5. `DeathManager.onDeath` stores `killerId` and evaluates the parsed condlists.

The condlists are evaluated with `registry.actor` as the first object and the dead stalker as the second object.

## Example

```ini
[logic]
active = walker@guard
on_death = death@guard

[walker@guard]
path_walk = guard_walk
path_look = guard_look

[death@guard]
on_info = %+guard_dead%
on_info2 = {=killed_by_actor} %+actor_killed_guard%
```

## Notes

- Missing `on_death` is allowed.
- If `on_death` names a section that does not exist, reset aborts.
- Use `mob_death` for monster death callbacks. `death` is the stalker scheme.
