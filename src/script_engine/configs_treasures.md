# Treasures

Treasure configs describe hidden stashes and the rewards assigned to them. XRF reads treasure data through
`TreasureManager` and related treasure utilities.

## Source files

Treasure manager source files live under:

- `src/engine/configs/managers/treasure_manager.ltx`
- `src/engine/configs/managers/treasures/treasures_zaton.ltx`
- `src/engine/configs/managers/treasures/treasures_jupiter.ltx`
- `src/engine/configs/managers/treasures/treasures_pripyat.ltx`

Script configs can also reference treasure inventory boxes, for example
`src/engine/configs/scripts/treasure_inventory_box.ltx`.

## Runtime behavior

Treasure data is loaded by the treasure manager. It tracks which treasures are available, found, or already looted, and
coordinates map spot display through map utilities.

When changing treasure behavior, check both:

- the manager config that defines treasure metadata and rewards;
- the script config or object section that represents the stash in the world.

## Editing checklist

- Keep treasure ids stable once saves can reference them.
- Keep reward sections valid and included.
- Check map spot behavior when a treasure should appear on the PDA.
- Test `TreasureManager` or treasure utility code when changing runtime behavior.
- Run `npm run cli verify ltx` after config edits.
