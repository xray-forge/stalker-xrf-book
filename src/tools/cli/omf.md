# OMF CLI

OMF commands inspect X-Ray motion files.

## `info-omf`

```powershell
xrf-tool info-omf --path ./meshes/example.omf
```

Options:

- `-p, --path <path>`: path to an `.omf` file. Required.

## Output

The command reads the motion file and prints:

- OMF version;
- motion count and motion names;
- total bone count;
- animation part names;
- bones assigned to each animation part.

## When to use it

Use `info-omf` when checking whether a motion file is readable, whether expected motions are present, or how motion
parts map to skeleton bones.
