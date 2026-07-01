# Lint

Linting is exposed as package scripts rather than a Commander command.

```powershell
npm run lint
npm run lint:strict
```

`lint` uses the standard ESLint config with cache under `target/eslint/cache.json`. `lint:strict` uses the stricter
repository config.
