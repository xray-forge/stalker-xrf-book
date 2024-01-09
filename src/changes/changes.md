# 🌡 Changes

## 🧪 General

- ~~Added new game bugs~~
- Optimized performance of scripts
  - Force instant initialization of all objects on game load/start to reduce lag duration
  - Memoize condlists
  - More efficient smart terrain jobs creation/checks
  - More game conditions checks where possible
  - Memoize client objects / simulator and other refs
  - Optimizing game quest state checks (one of most frequent game engine calls)
- Removing dead code / assets

## 🧪 Gameplay

- Adding gameplay options for OXR (interface rendering, loot simplification etc.)
- [optional] Treasures will have different color based on loot price (green, blue, purple, orange)
- [optional] Alternative start location
- [optional] Display map marks only after visiting
- [optional] Allow traveling only to known places
- [optional] Weapons/armor/helmets with random upgrades
- Improving and optimizing game AI/logics
  - Improving game schemes (wounded, loot collection, combat danger checks etc.)
- Physical boxes (wooden, iron) can drop loot with some chance based on difficulty/level similar to SHoC/CS

## 🧪 Graphics

- Added different variants of fullscreen mode rendering
- Added grass height configuration
- Added grass radius configuration
- Added rendering fps limit configuration
- Updated weather system
- Removed controls related to game patch download
- Enabled OXR screenspace/grading shaders

## 🧪 Build pipeline

- All the modules are separated and sorted with folders
- Added NodeJS based CLI
  - Added shared commands for intellij based IDEs 
- Added workflows to run tests/checks/build on repository commits
- Added tools for codebase linting/formatting (ltx/js/ts)
- Added tools for game assets compression
- Added tools for game translation
- Added tools for simplified engines management / testing with different variants of game engines
- Added tools for forms generation based on JSX
  - Adding type definitions and sharable basic components for custom UI rendering
- Added tools for game packages creation (build custom game with one command)
- Added tools for asset management (different locales etc.)
- Added tools for game profiling/debugging/development
  - Advanced logging (game, cli)
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
  - Game UI debugging tools
- Added unit testing with coverage checker
  - Added `fengari` lua VM for direct lua functionality checks
- Added loadouts presets for generating character profiles / loot

## 🧪 Modding

- Added game engine documentation
- Added game engine typing and according checks
- Added tools for generation of `html` file with game conditions/effects documentation based on JSDoc
- Fully rewritten game script engine with typescript
- Adding extensions support [WIP]
  - UI to reorder / enable / disable extensions
  - API to work with community extensions
- Added events management system with numerous game callbacks
- Added lua `marshal` lib and typing for it
- Added lua `lfs` lib and typing for it
- Added custom lua extensions lib and typing for it (based on CoC)
- Added fixtures for game API testing
- Added support for `ts` based `ltx` configs building (allows build-time logics and generation)
- Added shared utils lib to reduce code duplication and simplify frequently used logics
- Added managers abstraction for game logics control
- Updated schemes abstraction for easier testing/extending/sharing/updating of game logics schemes
- Add support for dynamic files with `marshal` lib, ability to save dynamic custom data without 16K limit

## 🧪 TODO

For todos check following git projects:

- [Script engine](https://github.com/orgs/xray-forge/projects/4)
- [Game engine](https://github.com/orgs/xray-forge/projects/6)
- [CLI and tools](https://github.com/orgs/xray-forge/projects/3)
