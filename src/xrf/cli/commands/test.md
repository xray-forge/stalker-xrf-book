# Test

Tests are run with Jest.

```powershell
npm test
npm test -- src/engine/scripts/register.test.ts
```

## Coverage

```powershell
npm run test:coverage
```

Coverage output is written under `target/coverage_report`.

## Type Checks

Use TypeScript checks separately from Jest:

```powershell
npm run typecheck
npm run typecheck:tests
```

The repository includes fixtures for mocked X-Ray APIs, Lua behavior, engine helpers, and CLI utilities under
`src/fixtures`.
