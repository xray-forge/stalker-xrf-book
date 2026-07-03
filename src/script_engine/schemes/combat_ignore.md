# combat_ignore

`combat_ignore` controls whether a stalker should accept a potential enemy while scripted logic is active. It is used
for base behavior and quest scenes where combat can be ignored until conditions change.

## Parameters

`combat_ignore` has no scheme-specific section fields in `SchemeCombatIgnore`.

The runtime state can receive logic overrides from the object registry. In particular, `combatIgnoreKeepWhenAttacked`
keeps combat ignore enabled when the actor hits the NPC.

## Runtime behavior

On reset, the scheme:

1. installs an enemy callback on the object;
2. subscribes `CombatProcessEnemyManager`;
3. enables the scheme state;
4. copies current logic overrides into the scheme state.

The enemy callback asks `canObjectSelectAsEnemy`. If an enemy can be selected and the NPC is assigned to a smart
terrain, the manager starts the smart terrain alarm. If the actor attacked a controlled smart terrain, the terrain
control is notified.

When the NPC is hit by the actor, the manager disables combat ignore unless overrides keep it active.

## Runtime sequence

1. Activation creates state without reading a dedicated section.
2. `add` creates `CombatProcessEnemyManager` and stores it on the state.
3. Reset installs `object.set_enemy_callback(...)`, subscribes the manager, enables the state, and copies overrides.
4. The enemy callback records actor combat state when the potential enemy is the actor.
5. If the enemy can be selected and the NPC belongs to a smart terrain, the terrain alarm starts.
6. Actor hits disable the state unless `combatIgnoreKeepWhenAttacked` is set.

## Example

```ini
[logic]
active = walker@base
combat_ignore = true

[walker@base]
path_walk = base_walk
path_look = base_look
```

## Notes

- The exact `combat_ignore` override syntax is parsed outside `SchemeCombatIgnore`; this page documents the scheme
  implementation that consumes the resolved override state.
- Enemy selection is rejected when the source and enemy are farther apart than the configured attack distance.
- `disable` clears the enemy callback and unsubscribes the stored manager action.
