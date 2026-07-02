# Execution flow

This page describes the normal flow from executable startup to active XRF gameplay. It is intentionally high-level:
forks can move engine internals around, but XRF depends on the same script entry points.

## 1. Executable startup

The engine initializes core services, resolves filesystem paths, loads configuration files, initializes logging, creates
the console, and applies startup command line arguments.

Important early choices include:

- filesystem config from `-fsltx`;
- console/user config from `-ltx`;
- compatibility mode from `-cop`, `-cs`, `-shoc`, or `-soc`;
- Lua JIT state from `-nojit`;
- post-init `start` or `load` commands from `-start` and `-load`.

## 2. Lua script engine initialization

The script engine initializes Lua, opens luabind exports, opens the standard Lua libraries used by the selected build,
adds game script paths to `package.path`, and loads script modules.

In XRF builds, `_g.script` is the root script entry point. It preloads:

- `register`;
- `bind`;
- `start`.

It also registers global externals for conditions, effects, dialogs, tasks, and callbacks.

## 3. Class and callback registration

The engine calls into `register.script` to register script-visible game classes and resolve class identifiers.

XRF registers:

- server object classes such as actors, stalkers, monsters, smart terrains, squads, items, weapons, anomalies, and
  physics objects;
- UI classes such as the main menu;
- engine callback functions exposed through global script paths.

The class registration step is what lets spawned engine objects construct the matching TypeScript-to-Lua classes.

## 4. XRF startup callback

The engine then calls `start.callback(isNewGame)`.

XRF uses this callback to:

- refresh class identifiers;
- register the ALife simulator and ranks;
- unlock system ini overriding;
- initialize managers;
- register scheme implementations;
- register extensions;
- emit `GAME_STARTED`.

Managers and schemes are available after this step.

## 5. Object creation and binding

The engine reads spawn data and creates server objects. When an object goes online on the client side, the engine asks
`bind.script` for the binder class.

XRF binds engine objects to classes such as:

- `ActorBinder`;
- `StalkerBinder`;
- `MonsterBinder`;
- `SmartTerrainBinder`;
- `RestrictorBinder`;
- `AnomalyZoneBinder`;
- `WeaponBinder`;
- `PhysicObjectBinder`.

The binder receives lifecycle calls from the engine and becomes the bridge between low-level object state and XRF
managers, schemes, and events.

## 6. Active gameplay loop

During gameplay, online binders receive updates, engine callbacks emit XRF events, managers react to those events, and
save/load packets serialize state. Offline ALife objects continue to exist on the server side even when no client-side
game object is active.
