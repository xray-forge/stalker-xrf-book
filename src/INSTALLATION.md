# 🌓 Installation

## Pre-requirements

- [Node.js](https://nodejs.org/en/) 14 or later
- [Stalker-COP](https://store.steampowered.com/app/41700/STALKER_Call_of_Pripyat/) game

## Setting up development environment

- DOWNLOAD [the game](https://store.steampowered.com/app/41700/STALKER_Call_of_Pripyat/)
- RUN `git clone https://github.com/xray-forge/stalker-xrf-engine.git` - clone repository
- RUN `cd stalker-xrf-engine` - cd to project folder
- RUN `npm install` - install all the dependencies
- RUN `npm run setup` - set up the project, install submodules
- RUN `npm run cli link` - link gamedata to the game folder
- RUN `npm run cli engine use release` - link open xray with game
- RUN `npm run cli build` - build gamedata to the destination
- RUN `npm run cli start_game` - start game and test changes

## Checking issues

`$ npm run cli verify project` - will check whether project is set up and ready to start developing

## Updating submodules

From time to time update of submodules is needed to load latest assets:

`git submodule update --recursive`
