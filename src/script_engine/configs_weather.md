# Weather

Weather configs define environment cycles, ambient sounds, fog, suns, thunderbolts, weather effects, and manager-level
weather selection. Runtime weather behavior is handled by `WeatherManager`.

## Source layout

Important source areas:

- `src/engine/configs/environment/environment.ltx`;
- `src/engine/configs/environment/weathers/*.ltx`;
- `src/engine/configs/environment/weather_effects/*.ltx`;
- `src/engine/configs/environment/ambients/*.ltx`;
- `src/engine/configs/environment/ambient_channels/*.ltx`;
- `src/engine/configs/environment/fog/*.ltx`;
- `src/engine/configs/environment/dynamic_weather_graphs.ltx`;
- `src/engine/configs/managers/weather_manager.ltx`;
- `src/engine/configs/managers/weather/weather_manager_levels.ltx`.

## Weather sections

The `$weather` schema is strict and includes fields for sky, fog, rain, sun, clouds, wind, water, ambient, thunderbolt,
and sun shafts. A weather cycle is built from time sections that point the engine at these values.

Weather effect sections describe temporary events such as surge, blowout, and psi storm effects. They reference
particles, sound, wind, and lifetime fields.

## Runtime manager

`WeatherManager` subscribes to actor update events and actor online events. It applies configured weather and exposes
debug information through the XRF debug panel.

When changing weather selection logic, update manager tests. When changing only LTX values, validate the configs and
check the result in game.

## Validation

Run:

```powershell
npm run cli verify ltx
```

Use the [debugging weather page](../debugging/weather.md) for in-game inspection and debug panel workflow.
