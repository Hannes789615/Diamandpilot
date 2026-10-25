# DiamondPilot 0.17.0 — build and verification status

## Changes in this snapshot
- Persists discovered diamond block positions and restores them for the same server/dimension.
- Clears cached positions when the server/dimension key changes, to avoid navigating to stale coordinates in another world.
- Removes cached positions when a later scan observes that a loaded block no longer contains diamond ore.
- Persists a bounded explored-region summary by loaded chunk and vertical section.
- Caps the stored diamond position cache at 12,000 entries and explored-region history at 5,000 entries.
- Saves settings through a temporary file and atomic replacement when supported.
- Snaps the initial tunnel direction to a cardinal yaw so tunnel checks and movement use the same direction.
- Monster retreat now checks loaded chunks, clear feet/head space, floor support, and fluids before moving; if no safe step exists, the bot stops.
- Includes `tools/verify_project.py` for offline structural checks.
- Adds Fabric Maven to dependency repositories so Loom can resolve Fabric Loader/API when network access is available.
- Corrects screen changes to call Minecraft’s `setScreen` API rather than the GUI overlay object.

## Still incomplete or unverified
- Built-in X-Ray rendering and normal-block transparency toggling are not implemented.
- Navigation through complex caves, vertical shafts, and arbitrary obstacles is experimental; the bot only tries up to three alternate tunnel turns before stopping.
- Server-side protection, anti-cheat, and multiplayer inventory behavior have not been tested.
- No successful Gradle build or in-game test has been run in this environment. No installable `.jar` is supplied.

## Verification performed
- `tools/verify_project.py`: passes all structural checks.
- `fabric.mod.json`: parses as valid JSON.
- ZIP archive integrity: checked after packaging.
- Java compiler probe with Java 21 found missing Minecraft/Fabric/Gson dependencies, so it could not compile the project. It reported no Java syntax-specific diagnostics, but this is not a successful compilation.

## Build requirement
The project targets Minecraft 26.3 and Java 25. This environment has Java 21 and no Gradle executable, and outbound DNS/network access is unavailable. Build and in-game verification must still be done in a Java 25 + Gradle environment.


## 0.13.0 work (2026-10-25)
- Added a client mixin that switches ordinary block render shapes to `INVISIBLE`, disables their solid-face occlusion, and reduces their light blocking while X-Ray is enabled. Diamond ore, bedrock, fluids, and block-entity blocks remain rendered.
- Connected the menu X-Ray button to the saved `xrayEnabled` setting.
- Registered the mixin configuration in `fabric.mod.json`.
- Structural verifier and JSON parsing pass.
- Not compiled or tested in Minecraft: mixin target/mappings and in-game rendering remain unverified. Transparent alpha rendering is not included.

## 0.14.0 work (2026-10-25)
- The J key now toggles the menu open and closed rather than opening a replacement screen repeatedly.
- X-Ray toggle requests a full level-render rebuild so cached chunk meshes should refresh immediately. This API call still requires compilation/runtime verification.
- Removed unused configuration flags for a transparent X-Ray mode that was not actually implemented, avoiding a misleading persisted setting.
- Updated build documentation to the Minecraft 26.3 toolchain requirement (JDK 25, Fabric Loom 1.18-SNAPSHOT, and Gradle 9.7.1).
- Structural checks pass; a real Gradle build and in-game validation remain blocked in this environment (Java 21 only, no Gradle executable, no dependency cache).


## 0.15.0 work (2026-10-25)
- Scanner revisits its current bounded search area periodically to catch chunks that load after an earlier pass.
- Search radius expansion now requires a minimum amount of loaded block data, avoiding rapid radius expansion when most of the volume is unloaded.
- Singleplayer scan caches are now keyed by the integrated world's name plus dimension, rather than treating every singleplayer world as the same world.
- Monster retreat movement now follows the same cardinal direction that was checked for safe feet/head space and floor support.
- Removed a no-op roof check in the tunnel logic.
- Added offline checks for the scanner refresh, singleplayer cache scoping, and checked retreat direction.
- Still no successful Gradle build or Minecraft runtime test is possible in this environment: Java 25/Gradle dependencies cannot be downloaded because outbound DNS/network access is unavailable.


## 0.16.0 work (2026-10-25)
- Updated the Loom plugin declaration to `1.18-SNAPSHOT`, matching Fabric's current 26.3 development guidance.
- Added a block ray trace check before attempting direct diamond mining, so ore behind an intervening block triggers approach/navigation rather than repeatedly trying to mine through a wall.
- Added offline checks for the Loom version declaration and direct-mining line-of-sight guard.
- This is still uncompiled: the environment has Java 21, no Gradle installation, and no outbound DNS, so API signatures and runtime behavior remain unverified.


## 0.17.0 work (2026-10-25)
- Added a GitHub Actions build workflow that selects JDK 25 and Gradle 9.7.1, runs the offline verifier, executes `clean build`, and uploads the resulting JAR as an artifact.
- Updated the mod version to 0.17.0.
- Confirmed official Fabric guidance for Minecraft 26.3: Java 25, Fabric Loader 0.19.5, Loom 1.18-SNAPSHOT, and Fabric API 0.163.0+26.3 on the development page; project retains the newer installed API version 0.165.0+26.3.
- Local build attempt remains blocked: Java 21 is installed, Gradle is absent, and `curl` cannot resolve `services.gradle.org` (DNS/network unavailable). GitHub Actions has not been run from this environment.
