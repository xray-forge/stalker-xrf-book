# Compress

Compresses built gamedata into database archives.

```powershell
npm run cli -- compress
```

## Options

- `-i, --include <targets...>`: include selected compression targets. Defaults to `all`.
- `-c, --clean`: clean the destination directory.
- `-v, --verbose`: print verbose logs.

Compression uses the paths configured in `cli/config.json` under `compression`.
