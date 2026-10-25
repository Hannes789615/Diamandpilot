# DiamondPilot 0.17.0 — experimental Fabric client mod

Minecraft Java 26.3 · Fabric · Modrinth instance source project.

## Included
- Client-side menu (default `J`) with Start, Pause/Resume, immediate Stop, scan radius, durability threshold, and an X-Ray toggle. The `J` key now toggles the menu open/closed.
- Does not auto-resume after game restart or disconnect.
- Incremental scan of diamond ore and deepslate diamond ore in block data already loaded by the client.
- Experimental two-block tunnel mining, local path search, and prioritization of known diamond veins.
- Conservative checks for fluid hazards, nearby monsters, pickaxe availability/durability, loaded chunks, floor support, and inventory pressure.
- Persistent counters, found ore positions, and a bounded explored-region summary, scoped to the current multiplayer server or singleplayer world and dimension.
- Atomic config replacement where supported.
- Conservative stuck detection that tries up to three alternate tunnel directions before stopping.
- Offline structural verifier: `python tools/verify_project.py`.
- GitHub Actions workflow (`.github/workflows/build.yml`) runs the structural checks, builds with JDK 25 + Gradle 9.7.1, and publishes the JAR as a workflow artifact when run in a connected GitHub repository.

## Important limitations
- **X-Ray implementation added:** a client mixin hides ordinary block models and disables their occlusion while enabled, while keeping diamond ore, bedrock, fluids, and block-entity blocks visible. The separate transparent-block rendering mode is not implemented. The X-Ray toggle requests a chunk-render rebuild so the visual change can take effect immediately.
- Complex route planning and recovery are experimental and can fail; do not rely on the automation to keep a character safe.
- The scanner periodically revisits its search volume so newly loaded chunks can be examined, and avoids expanding the radius when too little of the requested area is loaded. It can only inspect block data the client has received. It cannot see into unloaded chunks or reveal server-hidden data.
- Multiplayer/server rules may prohibit automated mining. Use only where permitted and test in a backup world first.
- This source snapshot has **not** been successfully compiled or tested in Minecraft. The mixin target and runtime behavior still need a real Gradle build and in-game test. There is no installable `.jar` yet.

## Build
Requires JDK 25 and Gradle 9.7.1 for the Minecraft 26.3 / Fabric Loom 1.18-SNAPSHOT toolchain. From this directory, run `gradle --no-daemon clean build`. A successful build should produce a JAR under `build/libs/`. The GitHub Actions workflow is included for a connected repository; it has not run from this local environment. This is an instruction, not a claim that the current source compiles.

See `BUILD_STATUS.md` for the exact verification limits.
