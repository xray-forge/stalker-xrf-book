# combat

`combat` configures scripted combat style for stalker NPCs. It selects a combat type through a condlist and installs the
planner hooks used by camper and zombied combat helpers.

## Parameters

### `combat_type`

Type: condlist. Optional. Default: `null`.

Resolves to a scripted combat type. Supported enum values in the TypeScript source are `camper`, `zombied`, and
`monolith`.

The section also supports common switch fields. They are parsed into `state.logic`, although the main combat behavior is
driven through planner evaluators and actions.

## Runtime behavior

On activation, the scheme:

1. marks combat overrides as enabled;
2. reads `combat_type`;
3. defaults zombied-community NPCs to `combat_type = zombied` when no field is provided;
4. resolves the selected combat type with `pickSectionFromCondList`;
5. stores it on the object registry state as `scriptCombatType`.

The `add` method registers `IS_SCRIPTED_COMBAT`, changes the base combat action precondition, and installs helper
actions for `combat_camper` and `combat_zombied`.

## Example

```ini
[logic]
active = walker@guard

[walker@guard]
path_walk = guard_walk
path_look = guard_look
combat_type = {+ambush_started} camper, nil
```

## Notes

- `combat` is usually combined with another active movement or state scheme. It changes combat planner behavior rather
  than replacing movement by itself.
- The `monolith` value exists in the enum, but the inspected implementation only wires specific helper actions for
  camper and zombied combat.
