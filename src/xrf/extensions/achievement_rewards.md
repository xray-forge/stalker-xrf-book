# Achievement Rewards

`Achievement rewards` periodically fills two actor reward boxes after the relevant achievements are earned.

The source lives under `src/engine/extensions/achievements_rewards`.

## Default state

The extension exports:

```ts
export const name = "Achievement rewards";
export const enabled = true;
```

It is enabled by default unless saved extension state overrides it.

## Behaviour

On registration, the extension subscribes to `EGameEvent.ACTOR_UPDATE`.

Each actor update checks these info portions:

- `detective_achievement_gained`;
- `mutant_hunter_achievement_gained`.

When the configured period has elapsed, it spawns reward items into the configured actor treasure box and emits a tip
notification.

## Rewards

The reward period is `12 * 60 * 60` game seconds.

Reward targets:

| Achievement   | Story box                 | Items                          |
| ------------- | ------------------------- | ------------------------------ |
| Detective     | `zat_a2_actor_treasure`   | medkit and antirads            |
| Mutant hunter | `jup_b202_actor_treasure` | armor-piercing ammo and shells |

The spawn count is fixed in the update code: detective rewards spawn with count `4`, and mutant hunter rewards spawn
with count `5`.

## Saved data

The extension persists two timestamps in dynamic extension data:

- `lastDetectiveAchievementSpawnAt`;
- `lastMutantAchievementSpawnAt`.

The timestamps are serialized through the time helpers and restored on extension load.

## Tuning

- Keep reward timing in `AchievementRewardsConfig.ts`.
- Keep achievement checks in `update.ts`.
- Add save/load fields when adding a new persistent reward timer.
- Keep notification captions aligned with translation ids.
