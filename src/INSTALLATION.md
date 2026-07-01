# Installation

This page describes a local development setup for `stalker-xrf-engine`.

## Requirements

- Windows development environment.
- [Node.js](https://nodejs.org/) with `npm`.
- Git with submodule support.
- Installed S.T.A.L.K.E.R.: Call of Pripyat.

The project can locate the Steam installation for app id `41700`. For a non-Steam copy, configure the fallback game path
in `cli/config.json`.

## Set Up the Project

Clone the engine repository and install dependencies:

```powershell
git clone https://github.com/xray-forge/stalker-xrf-engine.git
cd stalker-xrf-engine
npm install
npm run setup
```

`npm run setup` initializes and updates the repository submodules. The important submodules are `src/resources` for base
game resources and `cli/bin` for bundled binaries and tooling.

## Link the Game

Run the project verification before linking:

```powershell
npm run verify
```

Link the built `target/gamedata` folder, logs, and game folder paths:

```powershell
npm run cli -- link
```

Switch to one of the bundled engine variants:

```powershell
npm run cli -- engine list
npm run cli -- engine use release
```

Use `engine rollback` if you need to restore the default game executable.

## Build and Start

Build the gamedata output:

```powershell
npm run build
```

Start the configured game executable:

```powershell
npm run cli -- start_game
```

The build output is written to `target/gamedata`. Do not edit files there by hand; edit sources under `src/engine` and
rebuild.

## Updating Submodules

Update submodules when resource or binary repositories change:

```powershell
git submodule update --init --recursive
```

## Local OpenXRay Builds

For a locally built OpenXRay executable, follow the OpenXRay Windows build guide, then configure or copy the executable
into the engine location used by the CLI. Use `npm run cli -- engine info` to inspect the currently selected engine.
