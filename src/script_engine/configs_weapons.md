# Weapons

Weapon configs live under `src/engine/configs/items/weapons`. They define base weapon sections, HUD sections, sounds,
ballistics, upgrade links, add-ons, ammo classes, and mounted weapons.

## Source layout

Important files and folders:

- `base.ltx` for shared weapon definitions;
- `index.ltx` for includes;
- `w_*.ltx` for individual weapon sections;
- `upgrades/*.ltx` for weapon upgrade trees;
- `weapon_upgrades.ltx` and `upgrades_properties.ltx` for shared upgrade data;
- `$scheme/weapons.scheme.ltx` for validation coverage.

## Validation schema

Weapon schemas are mostly strict. They model common sections such as:

- `$item_weapon`;
- `$item_weapon_hud`;
- `$item_weapon_sounds`;
- `$item_weapon_attachable`;
- `$item_weapon_params`;
- `$item_weapon_grenade`;
- `$item_weapon_knife`;
- `$weapon_mounted`.

If a valid weapon field fails validation, update the narrow matching schema instead of disabling strict validation for
the whole weapon category.

## Runtime links

Weapon configs reference assets and other config sections:

- `visual`, `item_visual`, HUD positions, and bones;
- sound aliases such as `snd_shoot` and `snd_reload`;
- particle aliases such as `flame_particles` and `shell_particles`;
- ammo sections through `ammo_class`;
- upgrade sections through `upgrades`, `installed_upgrades`, and `upgrade_scheme`;
- add-on sections for scopes, silencers, and grenade launchers.

Check all referenced sections and assets when adding a weapon variant.

## Editing checklist

- Compare against a nearby weapon with the same weapon class.
- Keep HUD and world model sections separate.
- Validate upgrade section names and include order.
- Run `npm run cli verify ltx`.
- Test script-side weapon utilities only when changing runtime TypeScript behavior.
