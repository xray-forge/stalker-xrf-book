# Treasures

Treasure configs describe hidden stashes and the rewards assigned to them. XRF reads treasure data through
`TreasureManager` and related treasure utilities.

Use this page when you need to add a stash, grant stash coordinates from a quest, or check why a marked stash does not
produce the expected reward.

## Source files

Treasure manager source files live under:

- `src/engine/configs/managers/treasure_manager.ltx`
- `src/engine/configs/managers/treasures/treasures_zaton.ltx`
- `src/engine/configs/managers/treasures/treasures_jupiter.ltx`
- `src/engine/configs/managers/treasures/treasures_pripyat.ltx`

Script configs can also reference treasure inventory boxes, for example
`src/engine/configs/scripts/treasure_inventory_box.ltx`.

Quest and restrictor scripts grant stash coordinates with the `give_treasure` effect. Examples exist in task configs and
script logic, including:

- `src/engine/configs/managers/tasks/tasks_zaton.ltx`;
- `src/engine/configs/managers/tasks/tasks_jupiter.ltx`;
- `src/engine/configs/scripts/jupiter/jup_b43_task_giver_restrictor.ltx`;
- `src/engine/configs/scripts/pripyat/pri_b36_sr_ahi_place_pda.ltx`.

## Runtime behavior

Treasure data is loaded by the treasure manager. It tracks which treasures are available, found, or already looted, and
coordinates map spot display through map utilities.

The script effect `xr_effects.give_treasure` calls `TreasureManager.giveActorTreasureCoordinates(...)` for each passed
treasure id. This grants coordinates; the treasure definition still controls the stash metadata and reward contents.

Physical treasure containers enter the manager through object binders. `ObjectPhysic` and `ObjectHangingLamp` call
`TreasureManager.registerItem(this)` when they are constructed, so the manager can connect spawned world objects to
treasure data.

When changing treasure behavior, check both:

- the manager config that defines treasure metadata and rewards;
- the script config or object section that represents the stash in the world.

## Example grant

This pattern appears in Jupiter quest logic. It grants one treasure once and then records that the reward was already
given:

```ini
[sr_idle@reward]
on_info = {+jup_b43_contract_brought_first_artefact -jup_b43_once_treasure_give_1} %=give_treasure(jup_hiding_place_5) +jup_b43_once_treasure_give_1%
```

Use an info portion guard when the same logic section can be evaluated more than once.

## Editing checklist

- Keep treasure ids stable once saves can reference them.
- Keep reward sections valid and included.
- Check map spot behavior when a treasure should appear on the PDA.
- Check the world object or inventory box section that represents the stash.
- Check quest scripts that grant the treasure with `give_treasure`.
- Test `TreasureManager` or treasure utility code when changing runtime behavior.
- Run `npm run cli verify ltx` after config edits.
