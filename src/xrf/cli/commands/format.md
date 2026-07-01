# Format

Runs custom formatting commands.

## LTX

Formats LTX files:

```powershell
npm run cli -- format ltx
```

Options:

- `-c, --check`: check formatting without rewriting files.
- `-v, --verbose`: print verbose logs.

The package script `npm run format` is broader: it runs Prettier, ESLint fix, and LTX formatting.
