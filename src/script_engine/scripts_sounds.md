# Sounds

Sound scripts load sound themes from LTX, play one-shot and looped sounds, save active sound state, and map engine sound
masks to script sound classes.

## Source Layout

| Source                                  | Purpose                                                            |
| --------------------------------------- | ------------------------------------------------------------------ |
| `src/engine/core/managers/sounds`       | Sound manager, config, sound story classes, playable sound objects |
| `src/engine/core/managers/sounds/utils` | Theme loading, story helpers, playback helpers                     |
| `src/engine/core/utils/sound.ts`        | Console volume helpers and sound-mask mapping                      |
| `src/engine/configs`                    | Generated and static sound config inputs                           |

## Sound Config

`SoundsConfig.ts` loads:

- `managers\sounds\script_sound.ltx`;
- `managers\sounds\sound_stories.ltx`.

`script_sound.ltx` is parsed into `soundsConfig.themes`. Runtime state is stored in:

- `soundsConfig.playing`: current one-shot sound per object id;
- `soundsConfig.looped`: looped sound themes per object id;
- `soundsConfig.managers`: story managers by id.

## SoundManager

`SoundManager` is registered during startup. It subscribes to:

- `DUMP_LUA_DATA`;
- `ACTOR_GO_OFFLINE`;
- `ACTOR_UPDATE`.

The main methods are:

- `play(objectId, name, faction?, point?)`;
- `stop(objectId)`;
- `playLooped(objectId, name)`;
- `stopLooped(objectId, name)`;
- `stopAllLooped(objectId)`;
- `setLoopedSoundVolume(objectId, name, volume)`;
- `saveObject` and `loadObject` for object-local sound state.

`play` rejects looped sound themes. `playLooped` requires a looped theme.

## Heard Sound Mapping

`mapSoundMaskToSoundType` converts xray `snd_type` bit masks into script enum values used by `hear` and danger logic.

Supported groups include:

- weapon sounds: `WPN_shoot`, `WPN_empty`, `WPN_hit`, `WPN_reload`, `WPN`;
- item sounds: `ITM_pckup`, `ITM_drop`, `ITM_hide`, `ITM_take`, `ITM_use`, `ITM`;
- monster sounds: `MST_die`, `MST_damage`, `MST_step`, `MST_talk`, `MST_attack`, `MST_eat`, `MST`;
- fallback: `NIL`.

## Volume Helpers

`getMusicVolume`, `setMusicVolume`, `getEffectsVolume`, and `setEffectsVolume` read and write xray console variables for
music and effects volume.

## Guidelines

- Use `SoundManager` for scripted playback so save/load and looped state stay consistent.
- Use `sound_idle` fields in schemes when a scheme already owns the playback.
- Do not call `play` with looped themes or `playLooped` with one-shot themes.
- Stop object sounds when objects go offline.
