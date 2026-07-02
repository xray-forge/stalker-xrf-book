# Dialog configs

Dialog configs define conversation XML and script predicates/actions used by dialog phrases. XRF keeps dialog data under
`src/engine/configs/gameplay` and dialog script externs under `src/engine/scripts/declarations/dialogs`.

## Source files

Core dialog XML files:

- `src/engine/configs/gameplay/dialogs.xml`
- `src/engine/configs/gameplay/dialogs_zaton.xml`
- `src/engine/configs/gameplay/dialogs_jupiter.xml`
- `src/engine/configs/gameplay/dialogs_pripyat.xml`

Dialog text lives in translation files such as:

- `src/engine/translations/st_dialogs.json`
- `src/engine/translations/st_dialogs_zaton.json`
- `src/engine/translations/st_dialogs_jupiter.json`
- `src/engine/translations/st_dialogs_pripyat.json`

## Runtime hooks

Dialog XML can call script functions through registered dialog externs. XRF registers these from
`src/engine/scripts/declarations/dialogs`.

When changing a dialog:

1. update the XML phrase flow;
2. update translation ids used by the phrases;
3. update or add dialog predicate/action externs when XML calls script;
4. test the dialog declaration code when behavior changes.

## Generated XML

Some gameplay XML is generated from `.tsx` sources. Dynamic XML sources export `create()` and are rendered with
`renderJsxToXmlText`.

Do not edit generated XML under `target/`. Change the XML source or TSX generator.
