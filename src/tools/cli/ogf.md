# OGF CLI

OGF commands inspect X-Ray model files.

## `info-ogf`

```powershell
xrf-tool info-ogf --path ./meshes/example.ogf
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
