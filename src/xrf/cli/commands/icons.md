# Icons

Packs and unpacks equipment icon textures and XML texture descriptions.

```powershell
npm run cli -- icons <command>
```

## Commands

- `icons unpack-equipment`: unpack equipment icons as separate files.
- `icons pack-equipment`: pack separate equipment icons into one DDS sprite.
- `icons unpack-descriptions`: unpack icons from XML texture descriptions.
- `icons pack-descriptions`: pack separate icons for XML texture descriptions.

## Options

All icon commands support:

- `-v, --verbose`: print verbose logs.
- `-s, --strict`: enable strict mode.

Description commands also support:

- `-d, --description <name>`: process a specific description file.
