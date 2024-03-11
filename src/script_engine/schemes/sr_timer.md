# sr_timer

Scheme is implementing timers on screen and allows using timed logic. <br/>
Example: in ShoC everything related to time near psy-labs, bloodsuckers lair, helicopters evacuation limits etc.

## Type

Restrictor scheme.

## ini parameters

### ❄️ type

Type of timer to activate. <br/>
Increment will count from 0 to value, decrement is backwards timer.

- Type: `enum [inc, dec]`
- Default: `inc`
- Example: `dec`

### ❄️ start_value

Start value of timer to count from. <br/>
Required for decrement timers.

- Type: `number`, `milliseconds`
- Default: `0` for `increment`, required for `decrement`
- Example: `0`, `1000`, `60000`

### ❄️ on_value

Logics descriptor of how to handle different values of timer.

- Type: `number | condlist`
- Default: `null`
- Example: `0 | {+some_info_portion} section@first, section@second`

### ❄️ timer_id

Identifier of used timer form element. <br/>
Provides XML selector for rendering in UI hud.

- Type: `string`
- Default: `hud_timer`
- Example: `hud_timer`

### ❄️ timer_id

Identifier of used timer form element. <br/>
Provides XML selector for rendering in UI hud.

- Type: `string`
- Default: `hud_timer`
- Example: `hud_timer`

### ❄️ string

Optional text value to display with timer. <br/>
Displayed in `hud_timer_text` XML form element

- Type: `string`
- Default: `null`
- Example: `st_custom_timer_label`, `not translated text`

## Usage

todo; <br/>
todo; <br/>
todo; <br/>

## Examples

### pri_a28_sr_evac.ltx

```ini
[sr_timer@got_one_minute]
type = dec
start_value = 60000
on_value = 0 | {+pri_a28_evacuation_end =actor_in_zone(pri_a28_scene_end_zone)} sr_idle@cut_1_save, %+pri_a28_helis_leave%
on_info = {-pri_a28_evacuation_end =actor_in_zone(pri_a28_scene_end_zone)} %+pri_a28_evacuation_end =scenario_autosave(st_save_pri_a28_evacuation_end)%
on_info2 = {+pri_a28_helis_leave !actor_in_zone(pri_a28_scene_end_zone)} sr_idle@cut_2_save
```

### zat_b57_gas_timer.ltx

```ini
[sr_timer@gas]
type = dec
start_value = 45000
on_value = 0 | sr_idle@end {!squad_exist(zat_b38_bloodsuckers_sleepers)} %+zat_b57_gas_running_stop +zat_b57_den_of_the_bloodsucker_tell_stalkers_about_destroy_lair_give%
```
