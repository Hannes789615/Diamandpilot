# DiamondPilot — complete agreed requirements

This is the consolidated list of the decisions made in the conversation. Items are desired behaviour, not a claim that every feature has already been implemented.

## Platform and interface
- Minecraft Java 26.3, Fabric, managed through Modrinth.
- Mod name: DiamondPilot.
- Compact dark menu with green accents.
- Menu open/close uses exactly one freely configurable Minecraft keybind (Options → Controls → Key Binds → DiamondPilot). No separate hotkeys are required for Start/Pause/Stop/X-Ray.
- Menu controls: Start, Pause/Resume, immediate Stop, X-Ray toggle, block-display setting, search-radius setting, pickaxe durability threshold.
- Status in menu: searching / paused / stopped / danger stop, found diamond-ore blocks, mined diamond-ore blocks, current search radius.

## Search and mining
- Start from the user's current position; user digs down to the starting level themselves.
- Tunnel is 2 blocks high × 1 block wide.
- Continue tunnelling straight ahead to load new chunks, then scan the loaded area again.
- Start with a 40-block search radius, expand gradually when needed; radius remains adjustable in the menu.
- Scan continuously while digging.
- Inspect loaded block data for diamond ore; visible diamond ore can additionally guide navigation. A texture pack by itself does not give the mod block coordinates.
- Only mine diamond ore; ignore other ores.
- Prefer the largest reachable diamond vein first, then go directly to the next known vein.
- Mine through blocks to reach a vein where a safe/reachable route exists; may navigate up/down to follow diamonds.
- If a vein is unreachable, remember it and continue tunnelling straight; optionally retry later only after route logic is implemented.
- Remember explored areas and avoid unnecessary rescans.
- Save stats and progress: diamonds/ore found, ore blocks actually mined, distance travelled, explored areas, last position.
- Separate found and mined counters.
- If inventory is full, drop only stone; never drop valuable items. If still full, stop and wait.
- Gravel and sand may be mined to continue.
- Do not place torches.
- If stuck in a tight hole, attempt an alternative route.
- Prefer fast mining, while checking safety continuously.

## X-Ray visual mode
- Built into DiamondPilot; user should not need a separate X-ray texture pack for the intended display.
- Manual toggle only; never automatically enabled on Start.
- Diamond ore remains normally visible (no artificial glow required).
- Normal blocks can be switched between invisible and transparent.
- X-ray is a visual feature, not a source of hidden chunk data. Only blocks whose data reaches the client can be identified.

## Safety and failure handling
- Stop immediately and wait for the user if lava is detected.
- Stop immediately and wait for the user if water is detected.
- If a monster is encountered, back away until the path is safe, then resume.
- Pickaxe durability threshold is freely configurable in the menu; stop when the threshold is reached.
- If the pickaxe breaks, stop and wait; do not switch tools automatically.
- Stop button cancels movement and mining immediately, without finishing the current vein.
- After a game/mod/server restart, stay inactive until the user presses Start.
- On disconnect/server crash, disable the bot and wait for manual restart; do not auto-reconnect.
- If an obstacle blocks a vein, mark it and continue straight.

## Build and testing requirements
- Compile against Minecraft 26.3/Fabric before describing the result as installable.
- Test menu/keybind first, then X-ray rendering, then ore scanning, then navigation and safety stop behaviour.
- Do not claim autonomous mining, X-ray, or safety behaviour works until actually tested in a controlled test world and then on the permitted private server.
