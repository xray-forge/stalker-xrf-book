# Debugging UI Forms

XRF UI forms are authored as TSX/XML sources and loaded at runtime through xray CUI classes. Debug UI problems by
checking both sides: the generated form structure and the runtime code that initializes named nodes.

## Source and Runtime Files

Most UI work touches one of these areas:

- form sources in `src/engine/forms`;
- reusable form components in `src/engine/forms/components`;
- runtime window classes in `src/engine/core/ui`;
- XML render/build helpers in `cli/utils/xml`;
- generated output under `target`, which should be regenerated instead of edited by hand.

When a runtime class calls `xml.InitStatic`, `xml.Init3tButton`, `xml.InitScrollView`, or another `CScriptXmlInit`
method, the named XML node must exist in the form source.

## Check the Form Path

Runtime code usually resolves a form by path:

```ts
const base: TPath = "menu\\debug\\DebugDialog.component";
this.xml = resolveXmlFile(base);
```

If a form does not appear, verify:

- the `base` path matches the TSX/XML source path;
- the build generated the matching XML file;
- every node name used by the runtime class exists in the form;
- both 4:3 and 16:9 variants were updated when the form has paired variants.

## Use Engine UI Debugging

OpenXRay includes UI inspection tools for layout and control rectangles. They are most useful for positioning issues,
wrong sizes, and overlapping controls.

The `show_wnd_rect_all` console command is available in XRF command constants and can help identify active UI control
bounds in debug builds:

```text
show_wnd_rect_all 1
show_wnd_rect_all 0
```

Some UI debug tools depend on the engine build. If the command has no effect, run with a mixed/debug-capable engine.

## Common Failure Points

- A runtime `Init...` call references a missing XML node.
- A section exists in TSX but is not registered by the runtime dialog.
- A button is visible but not registered with `Register` and `AddCallback`.
- Text or controls fit in one aspect ratio but not in the paired 16:9 form.
- The generated XML in `target` is stale after changing the TSX source.

## References

- [OpenXRay UI debugger article](https://github.com/OpenXRay/xray-16/wiki/%5BEN%5D-Game-Editor#ui-debugger)
