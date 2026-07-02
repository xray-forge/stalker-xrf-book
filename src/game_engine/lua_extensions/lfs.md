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
