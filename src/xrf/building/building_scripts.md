# 🥑 Building scripts

Building scripts step is serving two purposes:

- build typescript based scripts and generate corresponding lua
- copy lua based scripts

## Watch mode

Allows TSTL compiler to watch over your project's source for changes, and rebuild typescript when they occur.
As result, there is no need in running `build` command every time scripts change.

To run scripts build in watch mode, following command can be used:

- `npm run watch:scripts`

## Typescript-to-lua

A generic TypeScript to Lua transpiler is used to power XRF project.
It allows writing typescript and transpiling it into lua.

- [docs](https://typescripttolua.github.io/)
- [git](https://github.com/TypeScriptToLua/TypeScriptToLua/)

For most questions up-to-date TSTL documentation should be used.

## Type definitions

Xray engine has set of API methods declared as global classes, enums and methods.
To access them from typescript codebase, type definitions are needed.

- [definitions package](https://github.com/xray-forge/xray-16-types)
- [definitions documentation](https://xray-forge.github.io/xray-16-types/index.html)
- to get up-to-date game exports, `-dump_bindings` game flag can be used

## Custom transformers

Since luabind offers very specific OOP functionality, which includes vtables and layer of pointer transformation
when exchanging data between lua and c++, built-in TSTL metatable based classes cannot be used.
To solve this problem, custom luabind class transformers are implemented.

Marking class with `@LuabindClass()` decorator tells transpiler to use luabind class system. <br/>
Instead of default metatables based classes code close to following is generated:

```lua
local Example = class("Example")(Base)

Example.__name = "Example"

function EvaluatorSectionActive.__init(self)
  Base.__init(self)
end

____exports.Example = Example
```

## `lualib_bundle.script`

Lualib is file providing shared libraries used in process of code transformation. It declares generic transformers for
typescript-lua interoperability. Various javascript standard methods are implemented here such as `Array.at`,
`Array.concat`, `Object.assign` and many others.
