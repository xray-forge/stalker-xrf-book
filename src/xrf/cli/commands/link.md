# Link

Links project output and logs to the configured game installation.

```powershell
npm run cli -- link
```

## Commands

- `link`: create project links.
- `unlink`: remove project links.
- `relink`: remove and recreate project links.

`link` and `relink` support `-f, --force` to override existing links.

## Examples

```powershell
npm run cli -- link
npm run cli -- relink --force
npm run cli -- unlink
```

Use `cli/config.json` to configure fallback game paths for non-Steam installations.
