# Forms

Forms are UI XML sources used by engine CUI classes. XRF keeps most form source in TSX under `src/engine/forms` and
builds it into XML.

Runtime UI classes live under `src/engine/core/ui`. They load XML, initialize controls, register callbacks, and update
the UI at runtime.

## Source types

| Source type                             | Build behavior                                           |
| --------------------------------------- | -------------------------------------------------------- |
| `src/engine/forms/**/*.tsx`             | Imported by the UI build and rendered to `.xml`.         |
| `src/engine/forms/**/*.ts`              | Imported when it exports a valid `create()` form source. |
| `src/engine/forms/**/*.xml`             | Copied as static UI XML.                                 |
| `src/engine/forms/textures_descr/*.xml` | Texture atlas metadata copied as UI XML.                 |

Dynamic forms must export `create()`. The UI build calls it and writes the result through `renderJsxToXmlText`.

## Components

Shared JSX components live under `src/engine/forms/components`. Common base components include:

- `XrRoot`;
- `XrElement`;
- `XrStatic`;
- `XrText`;
- `Xr3tButton`;
- `XrCheckBox`;
- `XrEditBox`;
- `XrScrollView`;
- `XrTab`;
- `XrTexture`.

Prefer these helpers over manually assembling repeated XML structures.

## Runtime loading

Runtime classes use engine UI helpers such as `CScriptXmlInit`, `CUIScriptWnd`, `CUIStatic`, `CUI3tButton`,
`CUIListBox`, and related CUI bindings.

When changing a form:

1. find the runtime class that loads it;
2. keep XML node names stable unless the runtime lookup is updated;
3. check paired 16:9 variants such as `name.tsx` and `name_16.tsx`;
4. update tests for runtime UI classes when element names or callbacks change.

## Validation

Run a focused UI build after form changes:

```powershell
npm run cli build -- --filter ui
```

Do not edit generated XML under `target/`.
