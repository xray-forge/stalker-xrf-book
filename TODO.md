# XRF todo

## 🧰 Main todos

- Create xrf.ltx config and place extended game configs in it
- Move animations/animstates etc to LTX files

## 🧰 Tech

- Custom lua loader to support dot separated files
- Add .dll as example and import it from TS?
- Add example C imports with TS (luajit)?

## 🧰 Requests to open x-ray

- Add callback notifying about game save to get filename
- With lua bindings generation include all call overrides when output TXT
- Export actor menu and actor menu item classes for overriding with lua
- Fix numerous calls to disk with menu, implement caching for character menu and fix lags when opening inventory
- XR_CPhraseScript -> allow function references as preconditions, not only string values
- XR_CPhraseScript -> allow function references to update text and react to dialogs
- Fix saving game / game encoding checks when system is using RU version of the game without installed locale OS / windows-1251 encoding
- For net packets add w_ctime and r_ctime methods
- map_has_object_spot -> add has one of
- iterate_online_objects is using 0xffff iterations, could iterate over simulator registered objects
