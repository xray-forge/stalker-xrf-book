# 🏗️ Debugging

todo; <br/>
todo; <br/>
todo; <br/>

## ️️🏗️ Using LUA/C++ debugger

To attach a debugger to Lua/C++ code, follow these steps:

- Download Visual Studio
- Install the [LUA debug](https://github.com/WheretIB/LuaDkmDebugger) extension for Visual Studio. (fixes [A](https://github.com/WheretIB/LuaDkmDebugger/pull/25) + [B](https://github.com/WheretIB/LuaDkmDebugger/pull/26) required)
- Set up the engine project by following the OpenXray instructions
- Link the game by running npm run link and targeting the folder of xrf
- Run the game in debug/release mode directly from Visual Studio

Note that it is not possible to debug TypeScript directly. <br/>
Instead, attach a breakpoint and observe the transpiled Lua code. <br/>
Additionally, it is not possible to debug luabind declared classes and userdata.

## ️️🏗️ Running custom script

todo;

## ️️🏗️ Debugging circular reference

todo;

## ️️🏗️ Debugging GOAP and logics

todo;

## ️️🏗️ Debugging weather

todo;


## ️️🏗️ Debugging UI forms

todo;
