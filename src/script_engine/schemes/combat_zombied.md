# combat_zombied

`combat_zombied` is an internal helper installed by the `combat` scheme. It provides simplified combat actions for
zombied-community stalkers: advance toward the enemy, shoot, and move toward danger sources.

## Parameters

`combat_zombied` has no standalone LTX fields in the current TypeScript implementation.

Zombied-community NPCs receive `combat_type = zombied` by default when `combat` is activated without an explicit
`combat_type`.

## Behavior

The helper adds `IS_COMBAT_ZOMBIED_ENABLED`. The current evaluator returns true when the object's community is
`zombied`.

It also adds two actions:

### `ZOMBIED_SHOOT`

Moves toward the current enemy's last seen position and uses raid/threat fire states depending on distance and
visibility.

### `ZOMBIED_GO_TO_DANGER`

Moves toward the best danger source, ignoring grenade movement targets and reacting to hits.

## Where it is installed

`combat_zombied` is added by the broader `combat` scheme; it is not normally used as an active section in LTX. Use
`combat_type = zombied` for intent, but also keep the NPC community as `zombied` because the evaluator checks community.

This helper is useful for simple zombie-style combat where the NPC should keep pressure on the enemy and react to danger
without cover or camper behavior.

## Example

```ini
[logic]
active = walker@zombie

[walker@zombie]
path_walk = zombie_walk
path_look = zombie_look
combat_type = zombied
```

## Notes

- `SchemeCombat` parses the `zombied` combat type, but the inspected `combat_zombied` evaluator itself checks the NPC
  community. Use zombied community data when relying on this behavior.
- `ZOMBIED_SHOOT` may play `fight_attack` on activation with a 25 percent chance.
- Use `combat_camper` or the normal `combat` flow when the NPC should use cover-like hide/look behavior instead.
