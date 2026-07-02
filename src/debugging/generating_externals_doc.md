# Generating Externals Docs

External functions are script functions that configs can call from condlists, effects, dialogs, and other game data. The
XRF CLI can scan declaration sources and generate an HTML reference for them.

## Run the Generator

From the XRF engine repository:

```powershell
npm run cli -- parse externals
```

The command scans declaration files under:

```text
src/engine/scripts/declarations
```

It skips `*.test.ts` files and `index.ts` files, groups declarations by parent folder, and writes:

```text
target/parsed/externals.html
```

Open the generated file in a browser when you need to inspect the currently exported external names.

## What Gets Documented

The generator reads TypeScript declaration sources, extracts external function metadata, renders the result as HTML, and
writes the rendered file with the project XML renderer.

Keep extern declarations close to the runtime implementation and tests. The generated page is only as useful as the
names, parameters, and comments present in the declaration files.

## Updating JSDoc

When adding or changing an external:

1. Update the declaration function and its JSDoc.
2. Keep the exported external name exactly aligned with the function registered for game configs.
3. Add or update the focused tests for the declaration.
4. Regenerate the externals HTML with `npm run cli -- parse externals`.

Prefer short comments that explain what config authors need: required arguments, optional arguments, side effects, and
what happens when the target object or parameter is missing.
