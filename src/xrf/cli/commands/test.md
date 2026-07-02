# Test

Tests are run through package scripts rather than a Commander subcommand. The project uses Jest with the config at
`cli/test/jest.config.ts`.

```powershell
npm test
```

## Common commands

| Command                         | Purpose                                                     |
| ------------------------------- | ----------------------------------------------------------- |
| `npm test`                      | Run the Jest suite.                                         |
| `npm test -- <path-or-pattern>` | Run focused tests.                                          |
| `npm run test:coverage`         | Run Jest with coverage output.                              |
| `npm run typecheck`             | Run TypeScriptToLua type checking without emitting scripts. |
| `npm run typecheck:tests`       | Type-check test sources with TypeScript.                    |

Coverage output is written under `target/coverage_report`.

## Examples

```powershell
npm test
npm test -- src/engine/scripts/register.test.ts
npm run test:coverage
npm run typecheck
npm run typecheck:tests
```

## Notes

Runtime tests use fixtures and mocks under `src/fixtures` for X-Ray APIs, Lua behavior, engine helpers, and CLI
utilities. Use focused Jest paths first when changing a specific manager, scheme, binder, or CLI helper.
