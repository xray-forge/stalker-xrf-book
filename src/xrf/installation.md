# 🌓 Installation

## 🧰 Pre-requirements

- [NodeJS](https://nodejs.org/en/) 14 or later
- [Stalker-COP](https://store.steampowered.com/app/41700/STALKER_Call_of_Pripyat/) game

## 💿 Start development

- DOWNLOAD [the game](https://store.steampowered.com/app/41700/STALKER_Call_of_Pripyat/)
- RUN `git clone https://github.com/xray-forge/stalker-xrf-template.git` - clone repository
- RUN `cd stalker-xrf-template` - cd to project folder
- RUN `npm install` - install all the dependencies
- RUN `npm run setup` - set up the project, install submodules
- RUN `npm run cli link` - link gamedata to the game folder
- RUN `npm run cli engine use release` - link open xray with game
- RUN `npm run cli build` - build gamedata to the destination
- RUN `npm run cli start_game` - start game and test changes

## 🧰 Check issues

`$ npm run cli verify` - will check whether project is set up and ready to start developing
