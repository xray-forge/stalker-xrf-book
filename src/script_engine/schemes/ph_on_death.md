# ph_on_death

`ph_on_death` switches a physical object when it receives a death callback. Use it for scripted reactions to destroyed
physics objects.

## Parameters

`ph_on_death` has no scheme-specific fields.

The section supports common switch fields. They are evaluated from the death callback.

## Runtime behavior

The manager subscribes to physical object death events. When the object dies and the object still has an active scheme,
it calls the common section-switching logic for the current `ph_on_death` state.

The death callback receives the dead object and optional killer object. The current manager does not inspect the killer;
conditions and effects in the switch fields define the response.

## Example

```ini
[logic]
active = ph_on_death@barrel

[ph_on_death@barrel]
on_info = ph_idle@dead %=give_info(barrel_destroyed)%
```

## Notes

- The implementation comments note that `disable` does not unsubscribe from the death callback because death is expected
  to happen once.
- The scheme does not apply damage, spawn particles, or play sounds by itself. Put those effects in the switch condlist.
