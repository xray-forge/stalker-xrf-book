# 🌡 Changes

## 🧪 General

- ~~Added new game bugs~~
- Optimized performance of scripts
  - Force instant initialization of all objects on game load/start to reduce lag duration
  - Memoize condlists
  - More efficient smart terrain jobs creation/checks
  - More game conditions checks where possible
  - Memoize client objects / simulator and other refs

## 🧪 Gameplay

- Adding gameplay options for OXR (interface rendering, loot simplification etc.)
- Treasures will have different color based on loot price (green, blue, purple, orange)
- Improving and optimizing game AI/logics
  - Improving game schemes (wounded, loot collection, combat danger checks etc.)

## 🧪 Graphics

- Added different variants of fullscreen mode
- Added grass height configuration
- Added grass radius configuration
- Added rendering fps limit configuration
- Updated weather system
- Removed controls related to game patch download

## 🧪 Build pipeline

- Added tools for simplified game localization
- Added tools for simplified game packages creation (build custom game with one command)
- Added tools for simplified asset management (different locales etc.)
- Added tools for game profiling/debugging/development
  - Advanced logging
  - Alife debugging
  - Console commands shortcut switcher
  - Game registry debugging
  - Items debugging
  - Lua profiling tools
  - Objects state/logics debug
  - Spawner
  - Task debugging
  - Teleportation
  - Treasures debugging
  - Weather debugging tools
- Added unit testing with coverage checker

## 🧪 Modding

- Added game engine documentation
- Added game engine typing and according checks
- Fully rewritten game script engine with typescript
- Adding extensions support [WIP]
  - UI to reorder / enable / disable extensions
  - API to work with community extensions
- Added events management system with numerous game callbacks

## 🧪 TODO

For todos check following git projects:

- [Script engine](https://github.com/orgs/xray-forge/projects/4)
- [Game engine](https://github.com/orgs/xray-forge/projects/6)
- [CLI and tools](https://github.com/orgs/xray-forge/projects/3)
