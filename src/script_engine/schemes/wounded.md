# wounded

`wounded` captures a stalker when health or psy-health reaches configured breakpoints. The NPC falls into a wounded
state, calls for help, can be helped by another stalker, and can auto-heal after a timeout.

## Parameters

The active logic section can point to a wounded configuration section:

| Field     | Location                             | Type         | Required | Description                                                                |
| --------- | ------------------------------------ | ------------ | -------- | -------------------------------------------------------------------------- |
| `wounded` | active section or `[logic]` fallback | section name | no       | Section used to configure wounded behavior. `nil` uses community defaults. |

The wounded configuration section supports:

| Field               | Type         | Required | Default           | Description                                                                             |
| ------------------- | ------------ | -------- | ----------------- | --------------------------------------------------------------------------------------- |
| `hp_state`          | wounded data | no       | community default | State/sound descriptor for HP wounds when the actor is not seen.                        |
| `hp_state_see`      | wounded data | no       | community default | State/sound descriptor for HP wounds when the actor is seen.                            |
| `psy_state`         | wounded data | no       | community default | State/sound descriptor for psy-health wounds.                                           |
| `hp_victim`         | wounded data | no       | community default | Victim descriptor stored in portable state.                                             |
| `hp_cover`          | wounded data | no       | community default | Parsed into state. The inspected manager does not currently store its processed result. |
| `hp_fight`          | wounded data | no       | community default | Controls whether the NPC can keep fighting while wounded.                               |
| `help_dialog`       | string       | no       | community default | Dialog used for wounded help interaction.                                               |
| `help_start_dialog` | string       | no       | `null`            | Start dialog set when the NPC becomes wounded.                                          |
| `use_medkit`        | boolean      | no       | community default | Allows medkit use after help or auto-heal unlocks it.                                   |
| `autoheal`          | boolean      | no       | `true`            | Allows automatic medkit unlock after wounded timeout.                                   |
| `enable_talk`       | boolean      | no       | `true`            | Stores whether talking is enabled while wounded.                                        |
| `not_for_help`      | boolean      | no       | community default | Marks the wounded object as not suitable for helper NPCs.                               |

## Wounded Data Syntax

Wounded data is parsed as repeated descriptors:

```text
hp|state_condlist@sound_condlist|hp|state_condlist@sound_condlist
```

Examples:

```ini
hp_state = 20|wounded_heavy_2@help_heavy
hp_fight = 20|false
psy_state = 20|{=best_pistol}psy_armed,psy_pain@wounded_psy
```

Each descriptor has:

| Part             | Description                                                              |
| ---------------- | ------------------------------------------------------------------------ |
| `hp`             | Breakpoint compared with current HP or psy-health in the `0..100` range. |
| `state_condlist` | Condlist resolved to the stalker state or special value.                 |
| `sound_condlist` | Optional condlist after `@`, resolved to the sound name.                 |

The parser selects the last descriptor whose breakpoint is greater than or equal to the current value before a higher
unmatched breakpoint stops the scan.

## Runtime behavior

On reset, the scheme resolves the wounded config section and parses the descriptor fields. Defaults differ for normal,
monolith, and zombied communities. Normal stalkers default to a wounded-heavy state with help sounds and medkit use.
Monolith and zombied defaults disable outside help.

The wound manager recalculates state on update and hit. Psy wounds are checked first. If no psy wound applies, HP wound
state and sound are selected from `hp_state` or `hp_state_see` depending on whether the NPC sees the actor. Fight and
victim results are stored in portable state.

When the wounded action starts, the NPC stops moving, disables trade, sets wounded state, registers as a wounded object,
and begins calling for help after the configured delay. If auto-heal is enabled and no helper unlocks medkit use, the
manager unlocks medkit use after the wounded timeout.

## Example

```ini
[logic]
active = walker@guard

[walker@guard]
path_walk = guard_walk
path_look = guard_look
wounded = wounded@guard

[wounded@guard]
hp_state = 25|wounded_heavy_2@help_heavy
hp_state_see = 25|wounded_heavy_3@help_heavy
hp_fight = 25|false
hp_victim = 25|nil
help_dialog = dm_help_wounded_medkit_dialog
use_medkit = true
autoheal = true
not_for_help = false
```

## Notes

- Wounded timing defaults are loaded from `schemes\wounded.ltx`: call delay `5000`, call period `5000`, wounded timeout
  `60000` if the config file does not override them.
- If the resolved wounded state is `nil` while the action is executing, the implementation aborts with a wrong wounded
  animation error.
