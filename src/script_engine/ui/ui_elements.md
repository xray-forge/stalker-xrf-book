# UI elements

X-Ray exposes CUI controls to Lua. XRF runtime UI classes initialize them from XML forms.

## Common classes

| Class                                      | Use                               |
|--------------------------------------------|-----------------------------------|
| CScriptXmlInit                             | Parse XML and create controls     |
| CUIWindow                                  | Base window                       |
| CUIDialogWnd                               | Dialog lifetime and visibility    |
| CUIScriptWnd                               | Script callbacks and child lookup |
| CUIStatic / CUITextWnd                     | Images and text                   |
| CUI3tButton / CUICheckButton               | Buttons and checkboxes            |
| CUIEditBox / CUITrackBar                   | Text input and sliders            |
| CUIListBox / CUIScrollView / CUITabControl | Lists, scrolling, and tabs        |

Call ParseFile before Init* methods. Keep XML node names, registered child names, and callbacks aligned. Parent windows own attached controls; lists own added items. Consult the X-Ray bindings for exact methods and value semantics.
