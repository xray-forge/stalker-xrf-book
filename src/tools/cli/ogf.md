# OGF CLI

OGF commands inspect X-Ray model files.

## `info-ogf`

```powershell
xrf-cli info-ogf --path ./meshes/example.ogf
```

Options:

- `-p, --path <path>`: path to an `.ogf` file. Required.

## Output

The command reads the model and prints available metadata:

- header version, model type, shader id, bounding box, and bounding sphere;
- texture and shader names;
- description chunk data when present;
- bones and parent names when present;
- motion references when present;
- child model texture and shader names for nested OGF data.

## When to use it

Use `info-ogf` to confirm that a mesh file can be parsed, to inspect texture references, or to compare model metadata
without opening a graphical tool.

## Workflow

Run `info-ogf` before texture or model packaging when you need to confirm what a mesh references. The command is
read-only: it does not rewrite chunks, normalize paths, or repair model data.

If a model fails to parse, first confirm the file is an OGF from the expected game version. Then compare the reported
failure with neighboring meshes from the same source archive.
