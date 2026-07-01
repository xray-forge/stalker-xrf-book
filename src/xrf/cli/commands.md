# Project Commands and Scripts

Run package scripts from the repository root:

```powershell
npm run <script>
```

Run CLI commands through the local wrapper:

```powershell
npm run cli -- <command>
```

## Package Scripts

- `setup`: initialize and update submodules.
- `verify`: run `verify project`.
- `build`: build scripts, configs, UI, translations, and resources.
- `pack:mod`: build a mod package.
- `pack:game`: build a game package.
- `watch:scripts`: rebuild scripts when TypeScript sources change.
- `typecheck`: run TypeScriptToLua type checking without emitting files.
- `typecheck:tests`: type-check test sources with TypeScript.
- `lint`: run ESLint.
- `lint:strict`: run the stricter ESLint config.
- `test`: run Jest.
- `test:coverage`: run Jest coverage.
- `format`: run Prettier, ESLint fix, and LTX formatting.
- `help`: print CLI help.

## CLI Commands

- `build`: build `target/gamedata`.
- `clone`: clone configured additional resource repositories.
- `compress`: compress built gamedata into archives.
- `engine`: inspect, switch, list, or roll back bundled engines.
- `format ltx`: format LTX files.
- `icons`: pack and unpack equipment icons and texture descriptions.
- `link`, `unlink`, `relink`: manage project links to the local game installation.
- `logs`: print the last lines from the linked game log.
- `open_game_folder`, `open_project_folder`: open configured folders.
- `pack`: create mod or game packages.
- `particles`: pack or unpack `particles.xr`.
- `parse`: parse directory trees or game externals.
- `spawn`: unpack ALife spawn files.
- `start_game`: start the configured game executable.
- `translations`: initialize, convert, and check translation files.
- `verify`: run project, gamedata, LTX, and particles verification commands.

Use `npm run cli -- <command> --help` for command-specific options.
