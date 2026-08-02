# Lint

Linting is exposed through package scripts, not a Commander subcommand. Use it for TypeScript, TSX, and CLI source
checks. The default command applies the full repository rule set, including unused import and local variable checks.

```powershell
npm run lint
```

## Commands

| Script         | Purpose                                       |
| -------------- | --------------------------------------------- |
| `npm run lint` | Run ESLint with the full repository rule set. |

The command stores its cache under `target/eslint/cache.json`.

## Examples

```powershell
npm run lint
npm run lint -- --fix
```

## Inputs and output

Lint reads TypeScript, TSX, and JavaScript sources from the repository and reports rule violations to the terminal. It
reuses the ESLint cache, so repeated runs after small edits are faster than a cold run.

## Related checks

Use `npm run typecheck` for TypeScriptToLua script type checks and `npm run typecheck:tests` for test TypeScript checks.
Use `npm test` for Jest.
