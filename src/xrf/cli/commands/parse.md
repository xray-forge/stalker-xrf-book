# Parse

Provides parsing utilities for generated data and documentation support.

```powershell
npm run cli -- parse <command>
```

## Commands

- `parse dir_as_json <path>`: parse a directory tree to JSON.
- `parse externals`: parse declared game externals for use in configs and docs.

`dir_as_json` supports `-e, --no-extension` to omit file extensions from JSON names.
