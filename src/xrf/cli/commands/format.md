# Format

`format` contains project-specific formatting commands. The current CLI subcommand formats LTX config sources with the
same formatter used by validation tooling.

```powershell
npm run cli -- format ltx
```

## Options

- `-c, --check`: check formatting without rewriting LTX files.
- `-v, --verbose`: print verbose formatter logs.

## Examples

```powershell
npm run cli -- format ltx
npm run cli -- format ltx --check
```

## Package script difference

The package script is broader:

```powershell
npm run format
```

It runs Prettier for JavaScript, TypeScript, TSX, and Markdown, then ESLint fix, then `npm run cli -- format ltx`. Use
the CLI command when you only want LTX formatting.
