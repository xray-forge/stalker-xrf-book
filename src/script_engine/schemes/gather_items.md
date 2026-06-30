# gather_items

`gather_items` controls whether a stalker can use the base item-pickup evaluator. It is generic stalker behavior, not an
active movement section.

## Parameters

### `gather_items_enabled`

Type: boolean. Optional. Default: `true`.

Enables item gathering for the current reset section.

## Runtime behavior

The scheme replaces the planner `ITEMS` evaluator with `EvaluatorGatherItems`. The evaluator returns true when
`gather_items_enabled` resolved to true and the engine reports that there are items to pick up through
`object.is_there_items_to_pickup()`.

## Example

```ini
[logic]
active = walker@post

[walker@post]
path_walk = post_walk
path_look = post_look
gather_items_enabled = false
```

## Notes

- Use this field on the active section that is passed to scheme reset.
- The scheme does not choose which items to pick up. It only controls whether the engine item evaluator is allowed to
  report available pickup work.
