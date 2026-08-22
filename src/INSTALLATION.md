# Installation

This guide prepares a local `xrf-engine` checkout, connects it to an installed game, and starts XRF.

## Requirements

- Windows.
- [Node.js](https://nodejs.org/) and `npm`.
- Git with submodule support.
- An installed copy of S.T.A.L.K.E.R.: Call of Pripyat.

The CLI looks for Steam app `41700`. For a non-Steam installation, set `targets.stalker_game_fallback_path` in
`cli/config.json`.

## Set up the project

Clone the engine repository and install dependencies:

```powershell
git clone https://github.com/xray-forge/xrf-engine.git
cd xrf-engine
npm install
npm run setup
```

`npm run setup` initializes the `src/resources` and `cli/bin` submodules used for assets, engines, and tools.

## Verify and link the game

Run the project verification before linking:

```powershell
npm run verify
```

Create development links for the game folder, `target/gamedata`, and logs:

```powershell
npm run cli -- link
```

List the bundled engines and select one:

```powershell
npm run cli -- engine list
npm run cli -- engine use release
```

`engine rollback` restores the original game executable.

## Build and start

Build the gamedata output:

```powershell
npm run build
```

Start the configured game executable:

```powershell
npm run cli -- start_game
```

For faster testing, the start command can create or load a session directly:

```powershell
npm run cli -- start_game --new --no-intro
npm run cli -- start_game --load quicksave
```

The build output is written to `target/gamedata`. Do not edit files there by hand; edit sources under `src/engine` and
rebuild.

## Update submodules

Update submodules when resource or binary repositories change:

```powershell
git submodule update --init --recursive
```

## Use a local OpenXRay build

For a locally built OpenXRay executable, follow the OpenXRay Windows build guide, then configure or copy the executable
into the engine location used by the CLI. Use `npm run cli -- engine info` to inspect the currently selected engine.
