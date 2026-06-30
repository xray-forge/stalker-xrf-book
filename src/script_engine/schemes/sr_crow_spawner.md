# sr_crow_spawner

`sr_crow_spawner` periodically spawns crow server objects at configured patrol paths while the total crow count on the
level is below a limit.

Use one active crow spawner per level unless the level intentionally needs multiple independent spawn sets.

## Parameters

### `max_crows_on_level`

Type: number. Optional. Default: `16`.

Maximum allowed `registry.crows.count` before spawning is throttled.

### `spawn_path`

Type: comma-separated strings. Optional. Default: empty string.

Patrol paths considered as crow spawn points.

The section also supports common switch fields.

## Runtime behavior

On activation, the manager initializes a cooldown entry for each spawn path. On update, if enough time has passed and
the current crow count is below the configured maximum, it tries the paths in random order.

A path can spawn a crow when:

- its cooldown has elapsed;
- its first patrol point is farther than 100 units from the actor.

The spawned server object section is `m_crow`. After a spawn, the selected path is put on a 10-second cooldown.

## Example

```ini
[logic]
active = sr_crow_spawner

[sr_crow_spawner]
max_crows_on_level = 7
spawn_path = zat_crow_spawn_1, zat_crow_spawn_2, zat_crow_spawn_3
```

## Notes

- `spawn_path` is parsed as a comma-separated list.
- If the crow count is already at the limit, the manager waits for the crow update throttle.
- Each path uses point `0` as the spawn position.
