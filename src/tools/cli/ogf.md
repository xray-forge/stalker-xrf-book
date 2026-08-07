# OGF CLI

OGF commands inspect X-Ray model files and rewrite their motion references.

## Commands

| Command                 | Purpose                                               | Writes files |
| ----------------------- | ----------------------------------------------------- | ------------ |
| `info-ogf`              | Print header, textures, bones, and motion references. | No           |
| `patch-ogf-motion-refs` | Replace the motion references stored inside a model.  | Yes          |

## `info-ogf`

```powershell
xrf-cli info-ogf --path ./meshes/example.ogf
```

Options:

- `-p, --path <path>`: path to an `.ogf` file. Required.

### Output

The command reads the model and prints available metadata:

- header version, model type, shader id, bounding box, and bounding sphere;
- texture and shader names;
- description chunk data when present;
- bones and parent names when present;
- motion references when present;
- child model texture and shader names for nested OGF data.

### When to use it

Use `info-ogf` to confirm that a mesh file can be parsed, to inspect texture references, or to compare model metadata
without opening a graphical tool.

The command is read-only: it does not rewrite chunks, normalize paths, or repair model data. If a model fails to parse,
first confirm the file is an OGF from the expected game version, then compare the reported failure with neighboring
meshes from the same source archive.

## `patch-ogf-motion-refs`

An animated model stores the paths of the OMF files it loads animations from. This command rewrites those paths, which
is what lets a model be moved to a different directory layout without re-exporting it from the SDK.

```powershell
xrf-cli patch-ogf-motion-refs --path ./meshes/wpn_ak74_hud.ogf --refs "dynamics\weapons\wpn_ak74\wpn_ak74_hud_animation"
xrf-cli patch-ogf-motion-refs --path ./hands.ogf --dest ./hands.patched.ogf --refs "dynamics\weapons\wpn_hand\hud_animation\*.omf"
xrf-cli patch-ogf-motion-refs --path ./hands.ogf --refs "first\animation" "second\animation"
```

Options:

- `-p, --path <path>`: path to an `.ogf` file. Required.
- `-d, --dest <dest>`: path to the resulting file. Defaults to rewriting the source file in place.
- `-r, --refs <refs>...`: one or more motion references to store. Required.
- `--dry-run`: report what the rewrite would produce without writing anything.

Paths use backslashes and omit the `.omf` extension, matching how the engine resolves them. A reference ending in
`\*.omf` is a wildcard: the engine loads every OMF in that directory.

### What is preserved

Only the motion references chunk is rebuilt. Every other chunk, including geometry, bones and IK data, is copied byte
for byte, so patching cannot alter the model itself.

The source file's chunk form is also preserved. Older models store references as one comma-separated string, newer ones
store a counted list; a file keeps whichever form it already used rather than being silently upgraded.

### Safety checks

Model geometry cannot currently be re-serialized from parsed data, so the command verifies rather than assumes:

- Before writing, it rewrites the file's **existing** references and requires the result to reproduce the source byte
  for byte. If it does not, the chunk copy would be lossy for that file and the command refuses to touch it.
- After writing, it reads the result back and requires the references to match what was requested.

If the second check fails, the write is undone: an in-place edit is restored from the original bytes, and a separate
destination file is removed. A model without a motion references chunk is refused before anything is written.

### When to use it

Use it when relocating animation banks, for example when consolidating per-weapon hand animations into one shared
directory and pointing every hands model at it with a wildcard. Confirm the result with `info-ogf`.
