# 🏗️ Engine debug (C++)

## ️️🏗️ Setup C++ engine project

Follow one of links to run custom game engine locally in debug mode:

- [open xray windows](https://github.com/OpenXRay/xray-16/wiki/%5BEN%5D-How-to-build-and-setup-on-Windows)
- [open xray linux](https://github.com/OpenXRay/xray-16/wiki/%5BEN%5D-How-to-build-and-setup-on-Linux)

## ️️🏗️ Using Lua/C++ debugger

To attach a debugger to Lua/C++ code, follow these steps:

- Use Visual Studio
- Install the [LUA debug](https://github.com/WheretIB/LuaDkmDebugger) extension for Visual Studio. (fixes [A](https://github.com/WheretIB/LuaDkmDebugger/pull/25) + [B](https://github.com/WheretIB/LuaDkmDebugger/pull/26) required)
- Set up the engine project
- Link the game by running npm run link and targeting the folder of xrf
- Run the game in mixed/release mode directly from Visual Studio

## ️🏗️ Limitations

- It is not possible to debug TypeScript directly. Instead, attach a breakpoint and observe the transpiled Lua code. <br/>
- It is not possible to attach to luabind declared classes and userdata (unfortunately, lua debug tools were not maintained in OXR for 10+ years)

## ️🏗️ todo

todo; <br/>
todo; <br/>
todo; <br/>

## ️🏗️ todo

todo; <br/>
todo; <br/>
todo; <br/>
