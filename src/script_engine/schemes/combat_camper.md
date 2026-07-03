# combat_camper

`combat_camper` is an internal helper installed by the `combat` scheme. It makes an NPC hide, remember the last seen
enemy position, and shoot only when the scripted combat type is `camper`.

## Parameters

`combat_camper` has no standalone LTX fields in the current TypeScript implementation.

Enable it through `combat_type` on a section handled by `combat`:

```ini
[walker@ambush]
path_walk = ambush_walk
path_look = ambush_look
combat_type = camper
```

## Runtime behavior

The helper adds two evaluators:

### `IS_COMBAT_CAMPING_ENABLED`

Returns true when the object registry `scriptCombatType` is `camper`.

### `SEE_BEST_ENEMY`

Returns true when the NPC sees its best enemy and stores the enemy position.

It also adds two actions:

#### `SHOOT`

Sets stalker state to `hide_fire` and looks at the best enemy.

#### `LOOK_AROUND`

Sets stalker state to `hide`, looks near the last seen enemy position, and periodically changes search direction.

## Where it is installed

`combat_camper` is installed by `SchemeCombat`. It relies on `registry.objects[object.id()].scriptCombatType` being
`camper`, which is set from `combat_type = camper` on the active combat-capable section.

Use it for ambush NPCs that should hold a position, hide, and shoot when they see the best enemy. Do not use it as a
movement scheme; path and base activity still come from the section that activates combat behavior.

## Notes

- `LOOK_AROUND` forgets the last seen position after `30_000` ms.
- The search direction changes after `10_000` ms initially, then every random `2_000` to `4_000` ms.
- Hit callbacks while looking around can refresh the last seen enemy position if the hit came from the best enemy.
- If an NPC never enters camper combat, check the active section's `combat_type` and whether normal combat activation
  ran for the object.
