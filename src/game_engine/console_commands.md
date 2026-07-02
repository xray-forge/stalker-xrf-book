# Console commands

Console commands are available after the engine console is initialized. They can be typed in the in-game console,
executed from config files, or triggered from scripts when the engine exposes command execution.

Most boolean commands use `on`/`off`. Some use `0`/`1`, numeric ranges, enum values, or custom command strings.

## Basic commands

| Command           | Use                                                               |
| ----------------- | ----------------------------------------------------------------- |
| `help`            | Print available console command help.                             |
| `quit`            | Exit the game.                                                    |
| `start ...`       | Start a game session with server/client arguments.                |
| `disconnect`      | Disconnect from the active session.                               |
| `save <name>`     | Save the current game.                                            |
| `load <name>`     | Load a save.                                                      |
| `load_last_save`  | Load the most recent save.                                        |
| `main_menu`       | Return to the main menu.                                          |
| `cfg_save <file>` | Save console settings to a config file.                           |
| `cfg_load <file>` | Load console settings from a config file.                         |
| `flush`           | Flush engine state where supported by the command implementation. |
| `clear_log`       | Clear the current log output.                                     |

## Script commands

`run_script <name>` reloads script paths and executes a script file through the engine script processor.

`run_string <lua>` executes a Lua string. In the inspected engine, the command preserves the original casing of the
string payload instead of lowercasing it with the command name.

Use these commands for focused debugging. For repeatable development workflows, prefer tracked scripts and XRF externals
instead of long console strings.

## AI debug commands

The inspected OpenXRay-style engine registers these AI and ALife debugging commands:

- `ai_debug`
- `ai_dbg_brain`
- `ai_dbg_motion`
- `ai_dbg_frustum`
- `ai_dbg_funcs`
- `ai_dbg_alife`
- `ai_dbg_goap`
- `ai_dbg_goap_script`
- `ai_dbg_goap_object`
- `ai_dbg_cover`
- `ai_dbg_anim`
- `ai_dbg_vision`
- `ai_dbg_monster`
- `ai_dbg_stalker`
- `ai_stats`
- `ai_dbg_destroy`
- `ai_dbg_serialize`
- `ai_dbg_dialogs`
- `ai_dbg_infoportion`
- `ai_dbg_node`
- `ai_dbg_sight`
- `ai_dbg_inactive_time`
- `ai_draw_game_graph`
- `ai_draw_game_graph_stalkers`
- `ai_draw_visibility_rays`
- `ai_animation_stats`

Older docs and forum posts sometimes spell `ai_dbg_frustum` as `ai_dbg_frustrum`. The engine command name is
`ai_dbg_frustum`.

## Render, UI, sound, and gameplay commands

Useful command families include:

- render toggles: `rs_stats`, `rs_fps`, `rs_fps_graph`, `rs_vis_distance`, `rs_cam_pos`, `rs_wireframe`;
- video settings: `vid_mode`, `vid_window_mode`, `vid_restart`, `renderer`;
- sound settings: `snd_volume_eff`, `snd_volume_music`, `snd_restart`, `snd_device`;
- HUD settings: `hud_draw`, `hud_info`, `hud_weapon`, `hud_crosshair`, `hud_crosshair_dist`, `hud_fov`;
- gameplay settings: `g_game_difficulty`, `g_language`, `g_sleep_time`, `wpn_aim_toggle`;
- Lua debugging: `lua_debug`, `lua_dump_depth`;
- ALife tuning: `al_time_factor`, `al_switch_distance`, `al_process_time`, `al_objects_per_update`, `al_switch_factor`.

Some commands are available only in debug builds or specific forks. Check the selected engine source before relying on a
command in documentation or tooling.

## XRF debug panel

XRF keeps a typed subset of console commands for the debug UI. If a command should be exposed from the panel, add it to
the engine constants first and verify the selected engine accepts the same name and value type.
