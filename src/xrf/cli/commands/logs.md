# Logs

`logs` prints the tail of the active engine log from the configured game logs folder.

```powershell
npm run cli -- logs
npm run cli -- logs 100
```

## Behavior

The optional argument is the number of lines to print. Invalid values fall back to `15`, and the command caps output at
`200` lines.

The command detects the log file from the configured game paths:

- when `bin/bin.json` exists, it expects `openxray_<username>.log`;
- otherwise it expects `xray_<username>.log`.

It reads from the real game logs folder. If you ran `link`, the same folder is also reachable through
`target/logs_link`.

## Examples

```powershell
npm run cli -- logs
npm run cli -- logs 50
npm run cli -- logs 500
```

The last example still prints at most `200` lines.

## Failure notes

If no active log is found, start the game once, check the configured game path, or run `npm run cli -- link` to create
the logs link for easier inspection.
