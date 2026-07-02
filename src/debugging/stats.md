# Stats

Use engine stats for frame/render timing and script stats for Lua-side call patterns. They answer different questions,
so enable the smallest view that matches the problem.

## Rendering and Frame Stats

Use these console commands while the game is running:

```text
rs_stats on
rs_fps on
rs_fps_graph on
```

`rs_stats` enables the engine statistics overlay. `rs_fps` shows the FPS counter. `rs_fps_graph` shows the FPS graph.
Use `off` to disable each toggle.

These commands are implemented by the engine, not by XRF scripts. Availability can depend on the engine build and
renderer configuration.

## AI Stats

For AI scheduler and planner timing:

```text
ai_stats on
```

The AI stats overlay is useful when you need to see whether a cost is coming from engine AI updates rather than an XRF
manager or scheme.

## Lua Profiling

The XRF `ProfilingManager` measures Lua call counts and manually marked profiling portions.

Open the XRF debug panel, switch to `general`, then use:

- start/stop profiling to attach or clear the Lua debug hook;
- log profiling stats to print hooked call counts;
- log portions to print manually captured `ProfilingPortion` measurements;
- refresh or collect memory to inspect Lua memory use.

For cleaner Lua hook stats, start the game with `-nojit`. LuaJIT can inline or alter execution in ways that make hook
counts less representative.

## Manual Profiling Portions

Use `ProfilingPortion` when you need to measure a specific code block:

```ts
const portion: ProfilingPortion = ProfilingPortion.mark("my_operation");

// Code to measure.

portion.commit();
```

The debug panel prints count, total duration, average duration, min, and max for captured portions.
