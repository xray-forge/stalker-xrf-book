# corpse_detection

`corpse_detection` is a generic stalker scheme for finding nearby lootable corpses. It adds planner logic that sends an
NPC to a corpse and plays the corpse-search state.

## Parameters

| Field                      | Type    | Required | Default | Description                                                         |
| -------------------------- | ------- | -------- | ------- | ------------------------------------------------------------------- |
| `corpse_detection_enabled` | boolean | no       | `true`  | Enables or disables corpse detection for the current reset section. |

## Runtime behavior

The scheme adds `IS_CORPSE_EXISTING` and a `SEARCH_CORPSE` action. The evaluator returns true only when the NPC is
alive, has no enemy, is not in danger, is not zombied, is not wounded, is not the cinematic actor visual, and a nearby
lootable corpse exists.

When a corpse is selected, the evaluator stores the selected corpse id, vertex id, and position in scheme state and
marks the corpse in portable storage so another NPC does not select the same corpse. The action sends the NPC to the
corpse, switches to `search_corpse` near the target, and plays `corpse_loot_begin` once.

## Example

```ini
[logic]
active = walker@camp

[walker@camp]
path_walk = camp_walk
path_look = camp_look
corpse_detection_enabled = false
```

## Notes

- `corpse_detection` is generic stalker behavior. It is usually controlled from the active logic section, not selected
  as `[logic] active`.
- Finalizing the search action frees the selected corpse marker.
