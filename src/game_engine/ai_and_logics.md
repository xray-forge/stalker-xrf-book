# AI and logics

XRF AI logic is built on the engine GOAP planners, engine callbacks, XRF schemes, and the XRF stalker state manager. The
engine still owns navigation, combat primitives, visibility, danger evaluation, animation execution, and the base
planner runtime. XRF adds script-side actions, evaluators, scheme state, and event routing.

Use this page to understand the structure. Use the [debugging AI page](../debugging/ai_and_logics.md) when you need
runtime overlays, planner dumps, or log output.

## Motivation planner

Each stalker has an engine motivation action planner. XRF modifies that planner when the stalker binder is
reinitialized. The setup adds script evaluators and actions that coordinate engine behavior with XRF-controlled
animation and logic state.

The motivation planner includes engine action ids for broad behaviors such as ALife, combat, anomaly, danger, gathering
items, smart terrain tasks, and death. XRF adds custom action ids for script activities such as animpoint, walker,
remark, sleeper, companion, smart cover, wounded, abuse, and state-to-idle transitions.

## State planner

`StalkerStateManager` owns a separate Lua-side action planner for stalker state control. It manages the target state and
drives sub-planners for:

- weapon state;
- movement;
- look direction;
- mental state;
- body state;
- animstate;
- animation;
- smart cover;
- locked states.

The state planner goal is to reach its `END` state after the required weapon, movement, mental, body, direction,
animstate, animation, and smart cover evaluators are satisfied.

## Evaluators

Evaluators answer yes/no questions for the planner. Examples include whether the stalker is already standing, walking,
in danger mental state, using a target weapon state, playing an animation, locked by animation, or inside a smart cover.

Evaluator ids are stable planner contracts. Changing ids can break planner graphs and debug output, even if TypeScript
still compiles.

## Actions

Actions change world state. XRF actions can strap or unstrap weapons, set movement, turn the stalker, switch mental and
body state, start or stop animations, and enter or leave smart cover.

Actions and evaluators used by engine planners must be luabind-visible classes. Keep `@LuabindClass()` on planner
classes that the engine action planner constructs or stores.

## Schemes

Schemes are the script logic layer loaded from LTX logic sections. A scheme activates state for a specific object and
section, subscribes handlers, and reacts to scheme events such as switching online/offline, death, hit, use, or
extrapolation.

The stalker binder wires schemes into object lifecycle:

- creates `StalkerStateManager` during reinit;
- sets up the state planner and motivation planner;
- initializes object logic on spawn;
- updates the state manager while the object is online;
- emits scheme events when the object switches offline, dies, is hit, or is used.

Keep scheme state in the object registry and clean it through the scheme lifecycle. Avoid storing per-object scheme
state only in module globals.
