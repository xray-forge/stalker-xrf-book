# Clone

Clones configured additional resource repositories.

```powershell
npm run cli -- clone <repository>
```

## Options

- `-l, --list`: print configured repository names.
- `-v, --verbose`: print verbose logs.
- `-f, --force`: overwrite an existing clone.
- `-s, --safe`: treat an existing clone as success.

## Examples

```powershell
npm run cli -- clone --list
npm run cli -- clone extended
npm run cli -- clone locale-ukr --safe
```
