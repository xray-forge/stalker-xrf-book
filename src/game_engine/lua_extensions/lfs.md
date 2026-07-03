# LFS

LFS is LuaFileSystem. It provides filesystem operations such as directory iteration, attribute inspection, directory
creation, links, locks, and current-directory changes.

XRF has TypeScript declarations for common LFS functions:

- `lfs.attributes(path)`;
- `lfs.dir(path)`;
- `lfs.currentdir()`;
- `lfs.chdir(path)`;
- `lfs.mkdir(path)`;
- `lfs.rmdir(path)`;
- `lfs.link(oldPath, newPath)`;
- `lfs.touch(path, atime, mtime)`;
- `lfs.lock(file, mode, start, length)`;
- `lfs.unlock(file, start, length)`;
- `lfs.symlinkattributes(path)`;
- `lfs.setmode(file, mode)`;
- `lfs.lock_dir(path, seconds)`.

The upstream library documentation is available at <https://lunarmodules.github.io/luafilesystem/>.

## Availability

The inspected engine script initialization does not open LFS as one of the default Lua libraries. Treat it as optional
unless your selected runtime package ships it.

For game paths, prefer engine filesystem helpers and configured path aliases. Use LFS for plain filesystem work only
when the runtime dependency is verified.

## Practical use

Use LFS for tools or controlled runtime packages where filesystem access is part of the environment contract. Avoid it
inside portable gameplay code unless the target executable is known to expose the module.

For game data lookup, prefer engine path aliases and file-system helpers because they follow mounted game paths and mod
layout rules. LFS works on process-visible filesystem paths; it does not know about engine virtual paths by itself.

## Verification checklist

- Check that `require("lfs")` succeeds in the exact runtime package you ship.
- Check path separators and working directory assumptions on the target platform.
- Keep save-game and game-state persistence on engine packet APIs instead of plain files unless the feature explicitly
  owns external files.
