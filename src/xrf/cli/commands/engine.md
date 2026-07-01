# Engine

Manages bundled game engine executables.

```powershell
npm run cli -- engine <command>
```

## Commands

- `engine info`: print the active engine state.
- `engine list`: print available bundled engines.
- `engine use <engine>`: switch to a bundled engine.
- `engine rollback`: restore the default engine backup.

## Examples

```powershell
npm run cli -- engine list
npm run cli -- engine use release
npm run cli -- engine info
npm run cli -- engine rollback
```

Package creation can also select an engine with `pack --engine <type>`.
