# CLI

The engine repository includes a local Node CLI. Run it from the repository root:

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

## Commands

It builds and packages the project, manages resources and engine binaries, links a local game, and handles common
format, spawn, particle, translation, and verification tasks.

See the [command list](./commands.md) for a quick index.

## Paths and output

Run commands from the repository root unless a page says otherwise. Most defaults are repository-relative and come from
`cli/config.json`.

Generated output belongs under `target/`: built gamedata, parsed helper files, coverage, packed archives, and package
output. Source edits belong under `src/engine`, `src/resources`, `cli`, or the relevant external resource repository.

## Installed command

`package.json` exposes the binary name `xrf`, but local development should prefer `npm run cli -- ...` so the command
uses the repository version and local dependencies.

If the package is installed globally, the equivalent shape is:

```powershell
xrf build
xrf verify project
```

## Configuration

Most CLI defaults live in `cli/config.json`: locale, resource roots, build source paths, target paths, compression
tools, package roots, and game executable settings.

When a command cannot find the game, resources, or generated output, check the command page first and then inspect the
matching config key in `cli/config.json`.
