# Luabind

Luabind is the bridge between C++ engine code and Lua scripts. The engine exports C++ classes, functions, enums, and
helpers into Lua, then game scripts create Lua-side classes that inherit from those bindings.

XRF TypeScript compiles to that Lua layer. A class that must be constructed or called by the engine needs to follow the
same luabind-visible shape after compilation.

## `@LuabindClass()`

Use `@LuabindClass()` on TypeScript classes that must be visible as Lua classes. Common examples include:

- binders that extend `object_binder`;
- action and evaluator classes used by GOAP planners;
- UI classes that extend engine CUI classes;
- server object classes registered through the factory.

The decorator preserves the class metadata expected by the TypeScript-to-Lua and luabind runtime path.

## Class names

Many registrations use the class `__name` field. XRF passes those names to engine registration code for game classes, UI
classes, and binder construction.

Changing a class name can therefore change runtime behavior even when TypeScript imports still compile. Treat class
renames as compatibility changes.

## Externals are separate from luabind

XRF also has an `extern(...)` helper. It writes values into `_G` or nested global tables so the engine and configs can
find script callbacks such as conditions, effects, task functions, dialog functions, and startup callbacks.

That is not the same as binding a C++ class. Use luabind classes when the engine constructs or calls class instances.
Use externals when a named global function or table entry must exist in Lua.

## `class`, `property`, and `super`

OpenXRay luabind exposes helper globals such as `class`, `property`, and `super`. They come from the luabind runtime,
not from XRF.

Modern XRF code usually does not call these helpers directly. TypeScript classes and `@LuabindClass()` generate the Lua
shape that the engine expects.

## Verification

Use `xray-16-types` to check TypeScript-visible API shape. For ambiguous behavior, check the engine binding code in the
selected `xray-16` fork, because some binding setters and object methods have engine-specific semantics.
