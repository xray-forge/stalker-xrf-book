# Original Start Position

`Original start position` changes the actor's start vertex and position for new games.

The source lives under `src/engine/extensions/original_start_position`.

## Default State

The extension exports:

```ts
export const name = "Original start position";
export const enabled = false;
```

It is disabled by default. Saved extension state can enable it.

## Runtime Behavior

The extension receives `isNewGame` from the extension registration flow.

When `isNewGame` is `true`, it calls:

```ts
set_start_game_vertex_id(287);
set_start_position(createVector(268, 20, 560));
```

When `isNewGame` is `false`, it does nothing. This prevents loaded saves from having their actor position changed.

## Save Data

This extension does not export `save` or `load`.

## Editing Notes

- Keep the `isNewGame` guard.
- Use engine start-position APIs only during new-game startup.
- Update tests if the vertex id or vector changes.
- Check the extension registration state when the code looks correct but the actor still starts elsewhere.
