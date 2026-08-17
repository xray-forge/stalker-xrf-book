# XRF changelog

This changelog records notable player-, modmaker-, and developer-facing changes in `stalker-xrf-engine`.

Latest check: xrf-engine `5875ddeb9a399ff371bc51151739e39888098a5a`.

## ~1.0.0

### August 2026

- **Database archives are packed by the XRF tools instead of xrCompress.** `compress` now calls `xrf-cli pack-archive`,
  which reads the same configuration dialect and writes archives straight into `target/db`, so the SDK binary is no
  longer part of a build. A group that fits one volume is now named `<target>.db` rather than `<target>.db0`. Archives
  holding mostly text pack somewhat larger, because the packer uses one LZO level where xrCompress used its slowest.

- **Script declarations are discovered and documented automatically.** Runtime callback, condition, effect, dialog, and
  task modules load from `gamedata/declarations`; builds emit an extern JSON manifest, and `verify externs` checks it
  against the declaration sources.

- **Lua modules load from loose files and packed archives.** The bundled engine resolves `require()` through X-Ray's
  virtual filesystem, with loose files overriding archived copies. This supports dynamic declarations and extensions in
  both development and packaged builds. The same rollup adds an inventory-info-removed callback and fixes sound resume,
  static spatial updates, duplicated HUD release callbacks, and release-build logging.

- **Strict payload validation moves to assembled gamedata.** `verify ltx` no longer accepts `--strict`; expensive
  payload checks run through `verify gamedata --strict`, while regular LTX typing, include, and inheritance checks
  remain available.

- **Random upgrades no longer affect trader inventory.** The enhanced-items extension now upgrades only weapons held by
  non-trader human NPCs. It no longer upgrades world items, outfits, helmets, or trader stock.

- **The start command can flush engine logs immediately.** Pass `--flushlog` when launching a test session to retain the
  final log lines after a hard crash; it may incur a performance cost while active.

- **Mounted weapons use the correct shell particles.** Mounted PKM weapons now reference the bundled shell effect, so
  their ejected casings render correctly.

- **Surge weather uses a bundled sky texture.** Surge phases no longer reference the unavailable
  `preblowout\\pre_blowout_0` texture.

- **Used particle effects ship with their textures.** The resource payload removes unused particle definitions and adds
  the blood, fire, smoke, spark, explosion, and distortion textures required by the remaining effects.

- **The single-player package trims multiplayer data without removing required definitions.** Multiplayer gameplay
  configs, unused UI assets, and server settings are omitted. Engine-required multiplayer screen and map-spot
  definitions remain because removing them caused crashes.

### July 2026

- **NPCs keep their animation state while changing smart-terrain jobs.** Reassigning an online NPC activates the new job
  directly instead of briefly clearing its active logic, preventing the ALife planner from resetting the NPC to idle
  between jobs.

- **Wounded NPCs settle their weapon before falling.** The wounded action selects the idle item and prevents weapon
  reselection until it ends, avoiding an interrupt that can restart the wounded animation.

- **Danger hearing has explicit reaction ranges.** NPCs respond to each danger type using squared-distance limits; enemy
  sounds are now considered within the configured 40-metre range instead of being disabled by a zero range.

- **Animpoint NPCs complete the requested turn before settling.** Reaching an animpoint uses a latched facing check.
  Leaving the point or changing its cover invalidates the completed turn.

- **Post-combat idle yields to wounds and active danger.** A wounded NPC is not captured by a post-combat animation, and
  a newly active danger state can take over instead of waiting for the post-combat delay.

- **Wounded NPCs do not enter danger from sounds.** The hearing handler declines weapon sounds before assigning danger
  inertia to an NPC already in a wounded state.

- **Weapon-sound handling rejects irrelevant work early.** The danger listener filters non-weapon sounds, non-creature
  sources, and out-of-range sounds before evaluating enemy relations.

- **The Zaton B52 bandit leader carries the required PDA.** The quest character's generated supplies now include
  `device_pda_port_bandit_leader`.

- **The CLI can start a targeted test session without navigating menus.** It supports a new game or named save, an
  optional difficulty, and optional intro skipping; this is a developer workflow, not a change to normal in-game start
  behavior.

- **The CLI can build on-demand in-game quest checks.** `checks list`, `build`, and `clean` manage flow scripts that
  observe quest progress, verify reached states, and report through the console, engine log, and a dedicated checks log.

- **Graphics presets load their shared settings with engine console syntax.** The renderer presets use `cfg_load` for
  the common AtmosFear settings instead of treating an LTX `#include` directive as a console command.

- **Quest helpers resolve the requested target and anomaly zone.** `destroy_object` keeps all story-target parameters,
  and the Zaton B29 artefact check searches the requested anomaly instead of the global artefact registry.

- **Psy post-process effects keep unique IDs.** Removing one active psy effect no longer lets the next effect reuse an
  ID that belongs to another active post-process.

- **Simulation assignment state is cleaned up safely.** Temporary squad-to-terrain assignments are cleared after use.
  Invalid or missing data follows fail-safe handling instead of leaving stale simulation state.

- **Missing music themes fail safely.** Music initialization validates unavailable theme data instead of continuing with
  incomplete state.

- **Travel dialogs reject NPCs without a valid squad.** Conversation predicates return unavailable for a missing squad
  instead of dereferencing it while checking routes, prices, or companion movement.

- **Squad-travel prices follow the simulated route.** Travel charges the server-graph distance between the squad and
  destination terrain, rather than the NPC's current world-position distance.

- **NPC sound state is independent.** XRF keeps playable sound state per NPC instead of sharing a theme instance among
  all users. Concurrent dialogue and ambient playback no longer overwrite one another.

- **Trade resupply state is initialized and refreshed consistently.** Traders retain the selected supply condition and
  refresh on the configured period instead of relying on incomplete descriptor state.

- **Mod-only packages can intentionally omit engine binaries.** `pack mod --skip-engine` no longer validates or requires
  a configured engine when it will not copy one, keeping a script/resource-only package independent of local engine
  selection.

- **Camp performers return to idle after finishing.** NPCs no longer remain stuck in guitar or harmonica animations
  after the corresponding camp story ends.

- **Localized interface text retains its accents.** French, Italian, Polish, and Spanish text no longer contains broken
  characters; the French, Italian, and Spanish options screens also label the world- and HUD-FOV controls.

- **Boars and flesh recognise each other as friendly.** Their relationship now matches vanilla's monster-relation
  configuration rather than treating the species as neutral.

- **Malformed quest condition lists are repaired.** XRF fixes broken vanilla condition-list data in the Zaton B7 and
  B20, Jupiter B8, and Pripyat underpass B400 quest logic so the conditions parse and evaluate as intended.

- **LTX validation covers game-script runtime schemes.** Common physical, restrictor, monster, stalker, weather, sound,
  task, and model sections declare strict `$scheme` types so malformed fields and condition lists can be caught before
  runtime.

- **Literal NPC-name checks.** NPC-name conditions now compare requested text literally while retaining the fast indexed
  loop. This prevents Lua pattern characters in a requested name from changing the match.

- **Dirty smart-terrain work has a global budget.** Reselection work is queued, deduplicated, and limited per frame and
  per second instead of letting every invalidated terrain run at once.

- **The Jupiter guide transition uses the intended fade and handoff.** First arrival as a visitor starts the missing
  black post-process, stops it after the welcome delay, and lets the guide enter its first-visit state when the journey
  condition applies.

- **Actor controls use persistent, ordered locks.** Overlapping systems such as cutscenes, surge survival, and anabiotic
  sleep keep their input/UI lock until release, so one cannot restore controls held by another.

- **Tracy profiling tools and engine variants are bundled.** XRF includes capture, viewer, export, and trace-import
  utilities together with `gold-tracy` and `release-tracy` engines. The older Lua-hook profiler was removed.

- **The LR300 sights align correctly.** Adjusted HUD offsets remove the rifle's misaligned aim view in both standard and
  16:9 layouts.

- **Smart-terrain maintenance adapts to distance.** Job maintenance is throttled by actor distance, while arrivals,
  departures, and state changes still mark the terrain for selection. The introducing change reports about 30% less
  per-frame job-processing work; it is not a general FPS benchmark.

- **Weather updates run on a 2.5-second actor cadence.** The weather manager uses the actor binder's throttled event
  instead of running on every actor update.

- **Unvisited restrictors check the actor at an interval.** Map-discovery restrictors accumulate elapsed time and test
  the actor position only at the configured interval instead of every update.

- **Physical-object callbacks follow the online lifecycle.** Hit, death, and use callbacks are installed once when an
  object comes online and cleared when it goes offline instead of being reassigned on every render update.

- **Scripted physical buttons no longer fail while logging their use.** Logger format arguments now match their
  placeholders in physical-object and treasure paths.

- **Resolved condition and effect functions are cached.** Parsed condition-list entries retain their resolved
  `xr_conditions` or `xr_effects` function reference after the first lookup.

- **Squad target outrank checks are staggered.** Simulation no longer performs every squad’s full target-outrank check
  at once; it uses staggered rechecks and caches terrain-assignment counts.

- **Squads reuse unchanged ALife tasks.** A squad retains its simulation task while its graph vertex is unchanged,
  avoiding repeated task allocation.

- **Game-graph lookups and distances have bounded caches.** Repeated level-name, vertex-level, and graph-distance
  queries reuse session-stable values; the distance cache has a fixed size.

- **Smart-terrain job selection is incremental.** Clean updates retain valid jobs and probe a bounded number of
  higher-priority candidates instead of reselecting every job.

- **NPC sound themes are registered on demand.** Sound setup defers theme registration until a sound is actually needed
  instead of preparing every possible theme for every spawned NPC.

- **The save dialog tests real file existence.** Existing-save detection uses the engine file object returned by
  `FS.exist`, so the overwrite warning follows the binding's actual result type.

- **X-Ray 16 SDK integration is upgraded to v2.** The earlier `xray16` package adoption expands to shared Lua helpers,
  mocks, and TypeScript aliases, and the remaining local copies are removed.

- **Texture quality exposes the full engine range.** The advanced-video slider always offers LOD values 0 through 4
  instead of raising its minimum from an address-space probe.

- **Scheme condition types are memoized.** Scheme switching records the parsed condition type after the first evaluation
  instead of repeating Lua pattern matching on every update.

- **Task updates are spread across time.** Task objects randomize the next update interval, avoiding synchronized
  re-evaluation bursts while keeping periodic checks.

- **Treasure statistics see condition-list emptying.** A treasure cleared by an `empty` condition list emits the same
  `TREASURE_FOUND` event as other collection paths, so event consumers record it.

- **Deimos cleanup removes the matching effectors.** The reset path now removes the camera and secondary post-process
  effectors by their correct IDs.

- **Cutscenes disable and restore the game UI.** Cutscene setup and cleanup pass the UI-lock state expected by the
  actor-input manager, including restoration of the remembered weapon slot.

- **Psy antennas play both channels.** The psy-antenna loop uses both the left and right sound objects, restoring the
  intended stereo effect.

- **Surges do not replay task-side effects when the hide task is disabled.** The surge manager records the task stage
  even when the configured section is `empty`, preventing duplicate alarms, sounds, and weather effects.

- **Task objects obey their update window.** A task with a known state skips functor evaluation until the next scheduled
  update instead of using the inverted time check; title and description changes are then applied together.

- **NPC torches turn off when their light is no longer needed.** Stalker torch state is applied for both outcomes, so
  daytime NPCs no longer keep their lamps on after a night-time or danger state.

- **Jupiter guide routes use the correct NPCs.** Travel from Jupiter now resolves the Zaton guide and Pripyat assistant
  by their actual story IDs instead of selecting the wrong character.

- **Disabled anomaly zones still update field switching.** Turning a zone off suppresses artefact respawn but no longer
  prevents its configured anomaly fields from cycling.

- **NPCs can use their configured cover chatter while approaching cover.** A `sound_idle` configured for a cover scheme
  now plays both while the NPC moves to cover and after it has arrived.

- **Close-combat state updates in one evaluation.** When enemy memory is already stale, the camper evaluator can enter
  and clear close-combat state in the same evaluation instead of exposing an incorrect extra planner update.

- **Patrols accept their full seven-NPC limit.** The patrol manager now permits seven registered NPCs and rejects only
  an eighth participant.

- **Exclusive smart-terrain jobs reserve a fallback slot.** When an exclusive job's `suitable` condition is false, the
  terrain retains an unconditional low-priority slot instead of leaving the NPC without a selectable job.

- **NPCs can hold smart cover during combat.** The missing combat action for `use_in_combat` is registered again, so the
  combat planner has an action to run without forcing the NPC out of its scripted cover.

- **Helicopters follow the intended patrol movement math.** The rewrite now uses vanilla's square-root velocity formula,
  ignores repeated waypoint callbacks, and keeps a dying helicopter registered until its normal object teardown.

- **Night predators wait until late night to hunt vegetarian monsters.** The `monster_predatory_night` simulation role
  uses a 21:00-to-day-start window instead of the broader 19:00 night window.

- **Zombied NPCs fire at their visible enemy.** The standing fire state now passes the selected enemy as its target
  instead of losing that target while starting the firing animation.

- **Critically wounded NPCs can still choose an enemy.** Critical wounds take precedence over a combat-ignore override,
  allowing the NPC to fight back.

- **Night vision is toggled from its actual state.** XRF checks the device's current state before changing it.

### May 2026

- **Bundled screen-space shader add-ons were removed.** The interactive-grass and shadow-cascade extensions, color
  grading presets, shader selectors, and their options-page controls are no longer part of the engine package.

### May 2025

- **Script builds can inject Tracy zones.** `build --inject-tracy-zones` instruments generated Lua for engine-level
  profiling with a compatible Tracy-enabled engine.

- **The CLI can verify assembled gamedata.** `verify gamedata` checks `target/gamedata` through the bundled XRF tools,
  with verbose and strict modes for deeper asset validation.

### March 2025

- **Generated NPC loadouts can use multiple sections and probabilistic attachments.** Modders can generate separate
  loadout sections and configure scope, silencer, and launcher attachment probabilities.

### January 2025

- **XRF adopts an expanded particle library with a round-trip editing workflow.** The CLI can unpack `particles.xr` into
  editable LTX files, repack those files, and verify both representations.

### December 2024

- **Dead creatures ignore late sound callbacks.** Stalker and monster binders decline hearing events after death instead
  of forwarding them into danger and scripted sound logic.

- **Weapon inventory icons match their items.** The Protecta and Winchester 1300 use corrected icon-grid positions in
  inventory and trade interfaces.

- **Teleport effects play their intended tinnitus sound.** The actor-teleport helper uses the engine's sound-path
  separator so its 2D sound resolves.

- **The debug panel can dump live Lua state.** Managers contribute bounded diagnostic data to
  `_appdata_\\dumps\\lua_data.json`, and the existing merged-`system.ini` dump uses the corrected file-writing path.

- **The runtime exposes extended engine callbacks.** OpenXRay/CoC hooks cover save/load completion, level changes,
  server-object removal, input, inventory focus and trade filtering, AI visibility, and weapon selection.

### August 2024

- **Translation workflows use the native XRF tools.** Translation builds moved to the bundled JSON/XML converter in
  July; initialization and validation followed in August, replacing their separate Node implementations.

### May 2024

- **Advanced video options expose renderer-specific controls.** The menu includes shadow-map quality and tessellation
  where supported, plus the always-active window setting.

- **AF3 weather cycles replace vanilla weather selection.** XRF selects `af3_*` weather sections from a dynamic
  AtmosFear graph, including moon-phase variants for clear and partly cloudy periods. It ships the matching weather,
  ambient-channel, and weather-effect configuration.

- **Original new-game spawn position.** XRF can set the vanilla start vertex and position only while creating a new
  game, leaving loaded saves untouched.

- **Equipment icons have a round-trip editing workflow.** The resource sprite is split into per-item source images, and
  the CLI can unpack or rebuild the combined equipment texture and its descriptors.

- **Jupiter scanner spots show the right artefact information.** After the scientist scanner reward, map hints update
  the existing spot with the current artefacts or an empty notice, including the corrected JUP B211 target and hint.

- **Map-display maintenance uses a shared five-second actor event.** Terrain and sleep markers use the throttled
  `ACTOR_UPDATE_5000` event instead of a per-actor-update callback and timer.

### March 2024

- **Bundled native XRF tools are available to the project CLI.** Their first integration provides reproducible LTX
  formatting and verification, including a non-writing formatter check mode for local use and CI. Later workflows use
  the same tools for translations, icons, particles, assembled gamedata, and extern manifests.

- **The CLI can unpack ALife spawn data.** `spawn unpack` converts the configured `all.spawn` into an inspectable output
  tree with path, destination, force, and verbose controls.

- **LTX verification understands typed project schemas.** `verify ltx` checks includes, inheritance, section types,
  required fields, and the initial `$scheme` declarations. Runtime game-script coverage expands in July 2026.

### February 2024

- **Engine switching tolerates broken links.** The CLI can replace a corrupted engine symlink instead of failing while
  trying to inspect its target.

### January 2024

- **Gameplay XML can be authored as typed TSX.** The config builder renders gameplay TSX into XML, allowing character
  profiles, dialogs, and related generated data to share components and helpers.

- **NPC character descriptions and loadouts use typed generators.** Reusable faction, profile, weapon, food, drug, and
  item presets replace hand-maintained XML spawn lists while retaining counts, condition, probability, and attachment
  flags.

- **Optional screen-space shader presets were added.** Players could select new or vanilla shader packages, choose color
  grading, and enable interactive grass and shadow cascades. These bundled add-ons were removed in May 2026.

- **Disabled extensions no longer crash save loading.** Extension restoration checks for an unregister hook before
  calling it, so a disabled module can load safely even when it does not define one.

- **Achievement reward caches replenish.** `Achievement rewards` extension records the Detective and Mutant Hunter
  awards in save data. Every 12 in-game hours after an award, it replenishes the configured medical supplies or
  armour-piercing ammunition in the Zaton or Jupiter reward box and posts a notification. Players can turn it off from
  the Extensions menu.

- **Saving does not create an inactive psy controller.** Save/load code serializes the psy-antenna manager only when it
  already exists, avoiding state changes caused solely by saving the game.

- **Signal lights restore their flight state after loading.** Scripted flare force, particles, and timing resume from
  saved state instead of being left partially initialized.

- **Generated character loadouts retain their first item.** The character-description builder emits a spawn section that
  preserves the first configured supply item.

### December 2023

- **XRF removes its authored multiplayer menu.** Multiplayer login, server, profile, and demo classes and forms are
  deleted, leaving the project-owned main menu focused on single-player. Engine-required multiplayer-named UI
  definitions remain a separate packaging concern.

- **Creatures restore a consistent online position.** Stalkers and monsters use an explicit spawn vertex, remembered
  offline vertex, or assigned smart-terrain job position when synchronizing after `net_spawn`.

### October 2023

- **Randomized upgrades.** The enhanced-items extension can add random upgrades when equipment first comes online.

- **Progressive smart-terrain discovery.** XRF initially hides smart-terrain map spots and blocks same-level travel to a
  terrain until the actor has visited it.

- **Extensions can be enabled and disabled from the menu.** Modules that allow toggling expose the control in the
  Extensions dialog, and disabled modules are skipped during registration.

- **An optional start-position extension is introduced.** Its first version applies an alternative position only to a
  new game started on Zaton. It was later reworked into the Original start position extension.

- **Post-combat idle has a shorter default delay.** NPCs wait 5 to 10 seconds after combat instead of vanilla's 10 to 15
  seconds. An explicit `post_combat_time` value still overrides the default.

- **Vendor-logo videos are skipped by default.** XRF disables the GSC, ATI, and AMD sequence; the intro renders an empty
  image item instead.

### September 2023

- **Translation tooling expands to eight locales.** English, French, German, Italian, Polish, Russian, Spanish, and
  Ukrainian sources can be initialized, merged with imported JSON, and checked for missing or invalid strings.

- **The development panel adds registry filters, profiling, and quest controls.** Developers can filter live objects,
  time selected runtime portions, and inspect or manipulate task state.

- **Verbose runtime systems can write dedicated logs.** Selected managers can write to their own file, either alone or
  alongside the main engine log.

- **World-object lifecycle events are available to runtime modules.** Actors, creatures, physical objects, zones, smart
  terrains, covers, helicopters, and inventory classes emit online/offline registration events.

- **The browser UI preview command is removed.** Form debugging moves to the bundled engine's ImGui and in-game
  development tools.

- **Advanced video options expose grass and window controls.** Players can change grass detail height and radius and
  select an engine window mode instead of using the old fullscreen checkbox.

- **NPC loading preserves the remembered level vertex.** When optional script save data omits a vertex, the binder keeps
  the stored offline position instead of replacing it with an empty value.

- **Psy antennas initialize sound with valid paths and intensity math.** The manager starts its sound objects, resolves
  the tinnitus assets with engine separators, and uses cubic power rather than a bitwise XOR expression.

- **Treasure icons show rarity.** Treasure map marks can distinguish common, rare, epic, and unique stashes instead of
  using only the generic treasure icon.

- **Off-level smart terrains maintain their jobs.** Job creation, update, and selection are no longer skipped merely
  because the actor is on another level.

- **Zombied danger actions require a living NPC.** The planner uses the alive property instead of the unrelated ALife
  property when deciding whether a zombied stalker can move toward danger.

- **The Jupiter B4 conversation uses a bound terrain check.** Its predicate calls the local smart-terrain helper instead
  of an unavailable external, preventing the dialogue path from failing.

- **Sleeping reliably opens its time-selection dialog.** The sleep manager creates the dialog when needed, fixing the
  path where the sleep UI was absent.

- **Field-of-view controls are available in the options UI.** XRF adds world-FOV (55 to 115) and HUD-FOV (0.40 to 1.00)
  sliders to advanced video options.

- **Game packages seed explicit FOV values.** `pack game` copies XRF's root `user.ltx`, setting initial world FOV to 80
  and HUD FOV to 0.7. Players can change these starter values in the XRF options UI.

### August 2023

- **XRF introduces a scripted dynamic-weather manager.** It selects weather-graph states, changes good and bad periods,
  coordinates surges and time changes, and saves and restores its state.

- **Engine declarations and build plugins move into the versioned `xray16` package.** The shared package replaces the
  earlier declaration submodule and becomes the base for the broader X-Ray 16 SDK integration.

- **Advanced video options have separate frame caps.** Gameplay and menu rendering use independent sliders for
  `rs_fps_limit` and `rs_fps_limit_in_menu`.

- **The bundled OpenXRay engine adds broader input and UI support.** The update includes keyboard and gamepad navigation
  for PDA, dialog, task-list, and message-box interfaces, plus renderer, audio, animation, grenade, and shutdown fixes.

- **The bundled engine includes an ImGui UI debugger and more script hooks.** Developers can inspect UI state with F10
  and use additional Lua-visible UI callbacks added by the same OpenXRay rollup.

- **NPC assistance can retarget while active.** When a corpse-search or wounded-help evaluator selects another target,
  XRF reissues movement instead of keeping the original corpse or wounded NPC.

- **Corpse-loot checks stop after the first valuable.** Before selecting a corpse to loot, XRF stops inventory iteration
  when it finds the first valuable item. Vanilla's evaluator scans the full inventory after setting its shared flag.

### July 2023

- **XRF supports modular gameplay extensions.** It discovers and registers modules from the extensions directory,
  persists their load order, passes each module its descriptor, and lets extensions load relative LTX files or override
  the runtime `system.ini`. The main menu initially managed ordering; enable/disable controls followed in October.

- **XRF save data has a separate sidecar.** Runtime-specific dynamic data is stored beside each `.scop` save, and
  runtime systems can react before and after save or load operations.

### June 2023

- **Optional resource repositories can be cloned from the CLI.** The command lists configured repositories and can clone
  a selected resource or locale payload with safe, force, and verbose modes.

- **Development links can be rebuilt in one command.** `relink` removes and recreates the configured game, gamedata, and
  log junctions, with an explicit force option when an existing target must be replaced.

- **Development commands locate Call of Pripyat through Steam.** XRF resolves Steam app 41700 automatically and uses the
  configured game path as a fallback.

- **The CLI can format LTX files.** The first formatter normalized line endings and surrounding whitespace.

- **Existing translation XML can be imported.** The translation workflow converts files or directories back into XRF
  JSON, detects or accepts the source encoding, and preserves multiline values.

- **The development panel gains world-control tools.** It can spawn squads and monsters, change actor relations, dump
  the merged `system.ini`, and teleport among levels, patrols, and positions.

- **Runtime events include game start and game-time timers.** Modules can subscribe to game start and register
  cancellable one-shot timeouts or repeating intervals driven by active game time.

- **Static resource builds skip unchanged files.** The builder compares source and target metadata before copying. It
  does not remove stale target files discovered by the comparison.

- **ALife catches up briefly after actor reinitialization.** XRF permits all ALife objects to update for the first three
  seconds after actor reinitialization, then restores the configured limit of 20 objects per update. This initializes
  the world promptly while retaining the normal update budget.

- **Nearby weapon fire can put NPCs on alert.** XRF passes heard sounds to the stalker danger controller. Eligible NPCs
  enter danger and move toward a nearby hostile shooter, or assist an ally engaging a mutual enemy. Vanilla's hearing
  callback only evaluates configured `on_sound` transitions.

- **Uncompressed game packages can copy the complete built tree.** The pack workflow defaults to building and
  compressing, with explicit opt-out flags for loose-development packages.

### April 2023

- **The CLI uses one structured command surface.** Build, engine, link, log, open, parse, start, and verification tools
  share subcommands, help text, and option parsing instead of being invoked as unrelated scripts.

- **The CLI can create compressed packages and complete game builds.** The first package workflow can build gamedata,
  compress database archives, and copy the configured engine and root payload into a distributable game.

- **Packaging distinguishes mods from complete games.** `pack mod` produces a mod-only layout, while `pack game`
  includes the configured standalone game payload. Complete game packages use the bundled `gold` engine by default
  unless another variant is selected.

- **The development dialog gains modular inspection tools.** Its sections inspect planner, inventory, registry,
  relation, scheme, weather, and ALife state and can spawn test items.

- **Runtime state uses a central save lifecycle.** Managers serialize through the save manager instead of embedding
  save/load orchestration in the actor binder.

- **Surge shelter searches are level-scoped.** The surge manager initializes the current level's cover list and compares
  squared distances instead of scanning every configured shelter for each lookup.

- **Gameplay options expose language and OpenXRay pickup controls.** Players can change language and toggle simplified
  pickup, multi-item pickup, and automatic magazine unloading after pickup.

- **The widescreen minimap has a corrected layout.** XRF supplies separate 16:9 frame and background dimensions instead
  of reusing the standard-aspect form.

- **Optimized packages can remove Lua logging.** `pack --optimize` builds scripts with Lua logger calls stripped from
  the generated output.

- **Game packages can ship compressed databases with only required loose runtime files.** When compression is selected,
  the packer copies database archives and an explicit runtime `gamedata` allowlist.

- **Build filters can target generated assets.** `build --filter` first targeted generated forms and configs; June
  expanded it to static UI, configs, and resources.

### March 2023

- **Runtime and CLI code can be tested outside the game.** The Jest harness supplies Lua and X-Ray stand-ins, while
  Fengari preserves important native Lua behavior for engine-facing TypeScript tests.

- **The project is renamed from XRTS to XRF.** Commands, documentation, artifact metadata, and repository references
  adopt the X-Ray Forge name.

- **Automated workflows build and validate XRF artifacts.** CI runs project checks and can publish assembled gamedata
  archives with source-revision metadata.

- **LTX configuration can be generated from typed TypeScript.** The builder supports sections, imports, root fields, and
  engine binding expressions, allowing generated configs to share constants and helpers.

- **Translations are built from a shared multi-locale source.** String tables are maintained in JSON and rendered to
  locale-specific engine XML with the required Windows-1251 encoding.

- **Scheme transitions use direct condition dispatch.** Active logic resolves each transition type and calls its mapped
  handler instead of traversing vanilla's long sequence of Lua-pattern branches.

### February 2023

- **The script runtime is authored in TypeScript.** XRF completed the migration of the vanilla Lua layer to TypeScript
  compiled through TypeScriptToLua, enabling source typechecking while still emitting Lua for X-Ray.

- **Decorated TypeScript classes can implement engine-facing luabind classes.** The compiler bridge emits the
  constructors, inheritance, methods, and properties expected by X-Ray.

- **Large binary and resource payloads are versioned independently.** Repository setup pins `cli/bin` and
  `src/resources` as submodules while the authored engine source remains in the main repository.

- **Builds can layer separately versioned resources.** Configured base, override, and locale roots are merged into the
  assembled gamedata. Locale selection and the option to omit additional override roots followed in April.

### January 2023

- **The CLI can verify a local XRF development setup.** `verify project` checks the project configuration, game
  executable, selected engine, gamedata and log links, and configured resource roots.

- **The CLI can serialize a directory tree as JSON.** The `parse` command provides a machine-readable inventory for
  resource and generated-data maintenance.

- **Runtime systems communicate through typed game events.** Managers and extensions can subscribe, unsubscribe, and
  emit lifecycle and gameplay events without direct coupling.

### December 2022

- **The XRF project CLI assembles authored sources into runnable gamedata.** Its first build pipeline compiles scripts
  and generates or copies configs, translations, UI forms, and resources into `target/gamedata`.

- **Builds include output metadata.** XRF records build flags and timing, host information, file counts, sizes, and the
  produced file list beside the assembled gamedata. Source-revision metadata followed in March 2023.

- **The CLI manages a local Call of Pripyat development install.** Developers can link gamedata and logs, start the
  game, inspect logs, open configured folders, and list, select, inspect, or restore bundled engines. The initial engine
  set included `release`, `gold`, and later `mixed`.

- **The main menu includes a development dialog.** F11 opens a debug panel with runtime-inspection and developer-control
  sections.

- **UI XML can be generated from reusable TSX forms.** Project-owned layouts are expressed as typed JSX components and
  rendered to engine XML during the build.
