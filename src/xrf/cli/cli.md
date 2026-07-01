# CLI

The XRF engine repository includes a local Node CLI. It is wired in `cli/run.ts` with Commander and is normally run from
the repository root.

Use the npm wrapper:

```powershell
npm run cli -- <command>
```

Common package scripts wrap frequently used commands:

```powershell
npm run build
npm run verify
npm test
```

## Command Groups

The CLI registers commands for building, cloning extra resources, compression, engine management, formatting, icons,
linking, logs, opening folders, packaging, parsing, particles, spawn files, starting the game, translations, and
verification.

See the [command list](./commands.md) for a quick index.

## Global Alias

`package.json` exposes the binary name `xrf`, but local development should prefer `npm run cli -- ...` so the command
uses the repository version and local dependencies.

If the package is installed globally or run through an npm executor, the equivalent command shape is:

```powershell
xrf build
xrf verify project
```

## Configuration

Most CLI defaults live in `cli/config.json`: locale, resource roots, build source paths, target paths, compression
tools, package roots, and game executable settings.
