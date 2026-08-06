# Debugging

Start with the narrowest tool that can explain the problem. Most XRF issues can be isolated with a focused build,
runtime logs, and the debug panel before a native engine debugger is needed.

## Choose the right tool

| Problem                                           | Start with                                          |
| ------------------------------------------------- | --------------------------------------------------- |
| Build output is missing or stale                  | The relevant build command and project verification |
| A script throws or behaves incorrectly            | [Logs](./logs.md) and a focused test                |
| An NPC uses the wrong scheme or animation         | [AI and logics](./ai_and_logics.md)                 |
| A form is missing or misaligned                   | [UI forms](./ui_forms.md)                           |
| Weather does not switch as expected               | [Weather](./weather.md)                             |
| Lua code is slow or uses too much memory          | [Performance statistics](./stats.md)                |
| A Lua API or engine callback behaves unexpectedly | [Engine debugging](./engine.md)                     |

The [XRF debug panel](./using_xrf_debug_panel.md) can inspect objects, managers, tasks, treasures, memory, profiling,
and merged runtime data without leaving the game.

## Basic workflow

1. Reproduce the issue with a known engine variant and save.
2. Rebuild only the affected target when possible.
3. Check the engine and Lua logs for the first relevant error.
4. Inspect live state with the debug panel or an engine overlay.
5. Reduce the problem to a focused test, config section, scheme, or engine API call.
6. Verify the fix with the same engine, save, and reproduction steps.

Do not build scripts with `--no-lua-logs` while investigating runtime behavior. Use a disposable save for console
commands, object mutation, teleportation, or forced weather changes.
