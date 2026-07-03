# Lint

Linting is exposed through package scripts, not a Commander subcommand. Use it for TypeScript, TSX, and CLI source style
checks.

```powershell
npm run lint
```

## Commands

| Script                | Purpose                              |
| --------------------- | ------------------------------------ |
| `npm run lint`        | Run ESLint with the standard config. |
| `npm run lint:strict` | Run the stricter ESLint config.      |

The standard command stores its cache under `target/eslint/cache.json`.

## Examples

```powershell
npm run lint
npm run lint:strict
npm run lint -- --fix
```

## Inputs and output

Lint reads TypeScript, TSX, and JavaScript sources from the repository and reports rule violations to the terminal. The
standard script reuses the ESLint cache, so repeated runs after small edits are faster than a cold run.

Use the normal lint command during documentation-adjacent code edits or focused implementation work. Use strict lint
before larger changes when you want the additional repository checks.

## Related checks

Use `npm run typecheck` for TypeScriptToLua script type checks and `npm run typecheck:tests` for test TypeScript checks.
Use `npm test` for Jest.
