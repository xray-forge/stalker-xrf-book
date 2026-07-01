# OMF CLI

OMF commands inspect X-Ray motion files.

## `info-omf`

```powershell
xrf-tool info-omf --path ./meshes/example.omf
```

Options:

- `-p, --path <path>`: path to an `.omf` file. Required.

The command prints version, motions, bone count, animation parts, and bones assigned to each part.
