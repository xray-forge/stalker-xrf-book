# Enhanced Items Drop

`Enhanced items drop (with upgrades)` adds random upgrades to weapons, outfits, and helmets when those items go online
for the first time.

The source lives under `src/engine/extensions/enhanced_items_drop`.

## Default State

The extension exports:

```ts
export const name = "Enhanced items drop (with upgrades)";
export const enabled = false;
```

It is disabled by default. Saved extension state can enable it.

## Runtime Behavior

On registration, the extension subscribes to:

- `ITEM_WEAPON_GO_ONLINE_FIRST_TIME`;
- `ITEM_OUTFIT_GO_ONLINE_FIRST_TIME`;
- `ITEM_HELMET_GO_ONLINE_FIRST_TIME`.

For each item, `onItemGoOnlineFirstTime` reads the owner id. Actor-owned items are skipped.

The extension then calculates a random chance and applies a different rate depending on whether the item belongs to:

- a trader;
- another owner;
- the world.

If the chance passes one of the configured thresholds, the extension calls `addRandomUpgrades`.

## Upgrade Tiers

The default config lives in `EnhancedDropConfig.ts`.

| Tier      | Chance | Upgrade count |
| --------- | ------ | ------------- |
| Common    | `20`   | `1`           |
| Rare      | `12`   | `3`           |
| Epic      | `6`    | `7`           |
| Legendary | `1`    | `30`          |

The final upgrade count includes random dispersion from `ADD_RANDOM_DISPERSION`.

## Save Data

This extension does not export `save` or `load`. It changes items when first-online item events are emitted.

## Editing Notes

- Keep chance and count tuning in `EnhancedDropConfig.ts`.
- Keep item filtering in `enhanced_items_drop_utils.ts`.
- Do not apply this extension to actor-owned starting items unless that behavior is intentional.
