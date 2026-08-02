# Enhanced Treasures

`Enhanced treasures` uses a treasure's type to choose its map icon.

The source lives under `src/engine/extensions/enhanced_treasures`.

## Default state

The extension exports:

```ts
export const name = "Enhanced treasures";
export const enabled = true;
```

It is enabled by default unless saved extension state overrides it.

## Behaviour

On registration, the extension sets:

```ts
treasureConfig.ENHANCED_MODE_ENABLED = true;
```

The treasure map helper uses this flag when choosing the map spot icon for a treasure descriptor.

## Map Spots

When enhanced mode is disabled, every treasure uses the generic `treasure` map mark.

When enhanced mode is enabled, `getTreasureMapSpot` maps treasure type to mark:

| Treasure type | Map mark          |
| ------------- | ----------------- |
| `COMMON`      | `treasure`        |
| `RARE`        | `treasure_rare`   |
| `EPIC`        | `treasure_epic`   |
| `UNIQUE`      | `treasure_unique` |

Treasure state itself is still owned by `TreasureManager` and `treasureConfig.TREASURES`.

## Saved data

This extension does not export `save` or `load`. Treasure manager state is saved by `TreasureManager`.

## Editing Notes

- Keep the extension toggle in `main.ts`.
- Keep treasure type-to-icon behavior in `map_spot_treasure.ts`.
- Keep treasure state changes in `TreasureManager`, not in this extension.
