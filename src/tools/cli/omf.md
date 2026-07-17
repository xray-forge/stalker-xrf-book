# OMF CLI

OMF commands inspect X-Ray motion files.

## `info-omf`

```powershell
xrf-cli info-omf --path ./meshes/example.omf
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

## Workflow

Run `info-omf` when debugging missing animations or checking a packed resource set. The command is read-only and prints
the parsed structure; it does not merge, split, or repair motion files.

If an expected animation is absent, check the source OMF first and then inspect the model or config that references the
motion name.

## Failure notes

The command expects a readable `.omf` file. It reports parsed metadata to stdout and leaves the source file unchanged.
When comparing animation packages, run the command on both files and compare motion names before checking higher-level
config references.
