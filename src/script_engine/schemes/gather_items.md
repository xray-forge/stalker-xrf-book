# gather_items

`gather_items` controls whether a stalker can use the base item-pickup evaluator. It is generic stalker behavior, not an
active movement section.

Use it on the section that becomes active for the NPC. The scheme is reset with that section and reads
`gather_items_enabled` from it.

## Parameters

### `gather_items_enabled`

Type: boolean. Optional. Default: `true`.

Enables item gathering for the current reset section.

## Runtime behavior

The scheme replaces the planner `ITEMS` evaluator with `EvaluatorGatherItems`. The evaluator returns true when
`gather_items_enabled` resolved to true and the engine reports that there are items to pick up through
`object.is_there_items_to_pickup()`.

If the field is omitted, `canLootItems` is set to true during reset. Setting it to false blocks this evaluator for that
active section only; it does not remove inventory logic from the engine or change item selection rules.

## Example

```ini
[logic]
active = walker@post

[walker@post]
path_walk = post_walk
path_look = post_look
gather_items_enabled = false
```

Use this on story NPCs that must stay in position, wounded or dialog scenes where looting would break staging, and
escort sections where the NPC should keep moving instead of reacting to nearby loot.

## Notes

- Use this field on the active section that is passed to scheme reset.
- The scheme does not choose which items to pick up. It only controls whether the engine item evaluator is allowed to
  report available pickup work.
- If a later section should allow looting again, omit the field or set `gather_items_enabled = true` there.
