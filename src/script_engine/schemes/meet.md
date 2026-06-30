# meet

`meet` controls how a stalker reacts to the actor at interaction distance: greeting sounds, idle animations, use
permission, dialog start, trade availability, abuse state, and interaction text.

Unlike active movement schemes, `meet` is a generic stalker scheme. The current active section points to a meet section
with a `meet = ...` field, and the meet scheme is reset whenever the active logic section changes.

## Parameters

Most fields are condlists. They can return values such as distances, animation states, sound ids, dialog ids, `true`,
`false`, or `nil`.

| Field                | Type               | Default source    | Description                                                                   |
| -------------------- | ------------------ | ----------------- | ----------------------------------------------------------------------------- |
| `close_distance`     | condlist number    | relation defaults | Distance for close meet state.                                                |
| `close_anim`         | condlist state     | relation defaults | Animation state used during close contact.                                    |
| `close_snd_distance` | condlist number    | relation defaults | Distance for hello and bye sound checks.                                      |
| `close_snd_hello`    | condlist sound     | relation defaults | Sound played when the actor first enters close sound distance.                |
| `close_snd_bye`      | condlist sound     | relation defaults | Sound played after hello when the actor is still inside far sound distance.   |
| `close_victim`       | condlist story id  | relation defaults | Object story id used as look victim for close animation.                      |
| `far_distance`       | condlist number    | relation defaults | Distance for far meet state.                                                  |
| `far_anim`           | condlist state     | relation defaults | Animation state used during far contact.                                      |
| `far_snd_distance`   | condlist number    | relation defaults | Distance for far sound checks.                                                |
| `far_snd`            | condlist sound     | relation defaults | Sound played while executing far meet state.                                  |
| `far_victim`         | condlist story id  | relation defaults | Object story id used as look victim for far animation.                        |
| `snd_on_use`         | condlist sound     | relation defaults | Use sound condlist stored by the scheme.                                      |
| `use`                | condlist value     | relation defaults | Controls actor use behavior. `self` starts talk directly.                     |
| `meet_dialog`        | condlist dialog id | relation defaults | Overrides the object's start dialog. `nil` restores the default start dialog. |
| `abuse`              | condlist boolean   | relation defaults | Enables or disables abuse state on the object.                                |
| `trade_enable`       | condlist boolean   | relation defaults | Enables or disables trading, unless the NPC is wounded.                       |
| `allow_break`        | condlist boolean   | relation defaults | Allows or blocks breaking the talk dialog.                                    |
| `meet_on_talking`    | boolean string     | relation defaults | Treats current talking as close meet contact when enabled.                    |
| `use_text`           | condlist string    | relation defaults | Overrides the interaction tip text. `nil` restores default use text behavior. |

The engine uses enemy defaults for hostile NPCs and neutral defaults for other relations. If the section is `no_meet`,
interaction is disabled by setting distances to `0`, animations and sounds to `nil`, and `use` to `false`.

## Usage

Reference a meet section from the active logic section:

```ini
[logic]
active = walker@guard

[walker@guard]
path_walk = guard_walk
meet = meet@guard

[meet@guard]
close_distance = 3
close_anim = talk_default
close_snd_hello = meet_hello
use = {=dist_to_actor_le(3)} true, false
meet_dialog = guard_dialog
trade_enable = false
```

To disable meeting for a section, point `meet` to the no-meet section used by the project config.

## Runtime behavior

The meet manager checks actor distance and visibility each update. It tracks close, far, and reset states. When the
actor moves beyond the reset distance, hello and bye flags are cleared.

When `meet_dialog` changes, the manager updates the object's start dialog. If the object is already talking, it can run
the actor talk dialog with the current break setting.

`use = self` starts the talk dialog directly when the actor can use the object and the screen is not black.

## Notes

- The reset distance is fixed in config at `30`.
- Wounded NPCs have trading disabled regardless of `trade_enable`.
- The meet action blocks ALife and idle reset while the actor is in meet contact.
