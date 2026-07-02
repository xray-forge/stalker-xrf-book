# Logs

Logs are the quickest way to inspect XRF runtime behavior. By default, `forge.ltx` enables debug mode, regular Lua
logging, and the separate Lua log file.

## Where Logs Go

The engine writes the main log into the game logs folder. XRF tooling can link that folder into the project as
`target/logs_link`.

Common files are:

- `openxray_<username>.log` when a custom OpenXRay binary descriptor is present;
- `xray_<username>.log` for the base engine name;
- `xrf_lua.log` when separate Lua logging is enabled;
- module-specific files such as `xrf_profiling.log` when a logger is configured with a file target.

The exact prefix depends on the logger configuration and the engine variant.

## Checking Logs

With a prebuilt engine:

1. Select the engine build you want to run.
2. Link the game folders with the XRF link command.
3. Start the game.
4. Inspect `target/logs_link`.

With Visual Studio:

1. Start the engine project from Visual Studio.
2. Check the Visual Studio Output window.
3. Check the same game log files if the message was written through the engine logger.

## Printing Logs with the CLI

The XRF CLI can print the end of the active engine log:

```powershell
npm run cli -- logs
npm run cli -- logs 100
```

The default count is `15` lines. Values are capped at `200` lines. The CLI looks for the linked logs folder and then
chooses `openxray_<username>.log` or `xray_<username>.log` depending on the detected engine.

## Writing Lua Logs

Use `LuaLogger` from runtime code:

```ts
const logger: LuaLogger = new LuaLogger($filename);

logger.info("Spawned object: %s", object.name());
logger.table(state);
logger.pushSeparator();
logger.printStack();
```

`LuaLogger` formats messages with the current engine time, file prefix, level, and formatted text. It writes to the
engine log unless a file-only logger is configured. When `separate_lua_log_enabled` is true, it also writes to the
shared Lua log file.

Use a file logger when the stream is noisy or belongs to a specific subsystem:

```ts
const logger: LuaLogger = new LuaLogger($filename, {
  mode: ELuaLoggerMode.DUAL,
  file: "profiling",
});
```

`DUAL` writes both to the named file and to the normal engine log.

## Flushing

Use the engine `flush` console command after generating important logs:

```text
flush
```

The profiling manager calls `flush` after printing profiling reports so the latest stats are persisted before you leave
the session.

## Build-Time Logging Flag

For performance or release-like builds, script compilation can strip Lua logger calls:

```powershell
npm run cli -- build -i scripts --no-lua-logs
```

Do not use that flag when you are investigating runtime behavior.
