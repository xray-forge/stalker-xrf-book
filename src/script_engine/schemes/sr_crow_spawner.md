# sr_crow_spawner

Scheme to describe logics of crows spawner. <br/>
From time to time spawns crows for declared paths based on maximal possible crows count.

Listens for game update event and synchronizes count of spawned crows with count of maximal crows. <br/>
Expects to have only one crow spawner restrictor per level.

## Type

Restrictor scheme.

## ini parameters

### ❄️ max_crows_on_level

Maximal count of crows spawned on level at once

- Type: `number`
- Default: `16`
- Example: `10`

### ❄️ spawn_path

Stringified list of patrols (paths) to spawn crows at.

- Type: `string`, `list`
- Default: `""`
- Example: `pri_crow_spawn_1, pri_crow_spawn_2, pri_crow_spawn_3, pri_crow_spawn_4, pri_crow_spawn_5`

## Usage

1) Create restrictor object in level editor
2) Assign sr_crow_spawner section as active logic in `ltx` file

## Examples

### zat_crow_spawner.ltx

```ini
[logic]
active = sr_crow_spawner

[sr_crow_spawner]
max_crows_on_level = 7
spawn_path = zat_crow_spawn_1, zat_crow_spawn_2, zat_crow_spawn_3, zat_crow_spawn_4, zat_crow_spawn_5
```

### jup_crow_spawner.ltx
```ini
[logic]
active = sr_crow_spawner

[sr_crow_spawner]
max_crows_on_level = 7
spawn_path = jup_crow_spawn_1, jup_crow_spawn_2, jup_crow_spawn_3, jup_crow_spawn_4, jup_crow_spawn_5
```
