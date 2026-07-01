# OGF CLI

OGF commands inspect X-Ray model files.

## `info-ogf`

```powershell
xrf-tool info-ogf --path ./meshes/example.ogf
```

Options:

- `-p, --path <path>`: path to an `.ogf` file. Required.

The command prints header data, bounds, texture and shader names, description chunks, bones, motion refs, and child
model texture data when those chunks are present.
