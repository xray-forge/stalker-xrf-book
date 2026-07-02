# Clone

`clone` downloads optional resource repositories configured in `cli/config.json`. The command clones into the parent
folder of the engine repository, next to `stalker-xrf-engine`.

```powershell
npm run cli -- clone <repository>
```

## Repository names

Use `--list` to print the configured names:

```powershell
npm run cli -- clone --list
```

Current configured names include:

- `extended`: full base gamedata assets for custom game packages;
- `locale-eng`: English locale resources;
- `locale-ukr`: Ukrainian locale resources;
- `locale-rus`: Russian locale resources.

## Options

- `-l, --list`: print configured repository names and exit.
- `-v, --verbose`: print verbose logs.
- `-f, --force`: remove an existing target folder before cloning.
- `-s, --safe`: treat an already cloned target folder as success.

`--force` and `--safe` conflict with each other.

## Examples

```powershell
npm run cli -- clone extended
npm run cli -- clone locale-ukr --safe
npm run cli -- clone extended --force
```

## Failure notes

The command fails when the repository name is missing, not configured, or already cloned without `--safe` or `--force`.
It runs `git clone`, so network and credentials errors come from Git.
