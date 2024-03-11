# 🍇 Extensions (WIP)

XRF engine allows adding custom modular extension to the game that can be toggled on/off from game menu any time.

## LTX configurations

Variant of modular game extensions with strictly defined interface and optional loading. <br/>
Extensions can load with root register method, use shared game utils and services.

## What can be done

- Optional loading for extensions
- Ordering and in-game configuration of extensions list
- Usage of shared utils, configs, managers and schemes
- Adding custom logics and configuration
- Overriding system ini sections/fields

## Todo / research

- Custom translations sources / build steps to prepare translations from extensions
- Custom configs / build steps to transpile extension configs
- Automated way to load system ini overrides and apply to existing system ini file
- Save data about active extensions and warn when loading game with invalid extensions list
- Add `requires` fields when extensions depend on each other
