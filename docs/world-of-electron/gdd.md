# World of Electron — Game Design Document

> **Status:** Prototype v0.1 — discussion draft, not an approved specification.  
> **Source:** [WorldOfElectronOld](https://github.com/Hyarius/WorldOfElectronOld) (private).  
> **Goal:** Reimagine the original project as a smaller multiplayer game built with modern Sparkle.

## 1. Original project: what is actually confirmed

The old CMake project describes **World Of Electron** as a **"Multiplayer Action-RPG SpaceShooter."** The current code contains a pixel-inspired cyan/gray main menu with **Join Game**, **Host Game** and **Quit**. The host form exposes **world name** and **chunk size** (8, 16, 32, 64). The join panel has no functionality. No character movement, combat, generated world, active game session, server browser, or multiplayer networking implementation was found in the repository.

These are **clues about the intended direction, not evidence of completed features**.

Relevant sources: [project description](https://github.com/Hyarius/WorldOfElectronOld/blob/main/CMakeLists.txt), [main menu](https://github.com/Hyarius/WorldOfElectronOld/blob/main/src/widget/panel/main_menu_panel.cpp), [host form](https://github.com/Hyarius/WorldOfElectronOld/blob/main/src/widget/panel/host_game_panel.cpp), [join panel](https://github.com/Hyarius/WorldOfElectronOld/blob/main/src/widget/panel/join_game_panel.cpp).

## 2. Proposed game vision

**A small, replayable, cooperative 2D space shooter.** Players pilot energy-powered craft through dangerous space sectors, fight enemy machines, collect energy and upgrade their craft. The action should be immediate, readable and satisfying even with just two players.

Design pillars:
- **Fun within minutes:** no mandatory grinding or complex setup.
- **Better together:** teammates have reasons to coordinate.
- **Short, replayable sessions:** each game has a clear start, challenge and ending.
- **Multiplayer first:** hosting and finding games are part of the core experience.
- **Small first release:** polish a single loop before adding vast worlds or RPG systems.

## 3. Proposed gameplay loop

1. Open a **separate launcher**, browse available hosted games, or create one.
2. Join a lobby and enter a sector with friends.
3. **Move, aim, shoot and dodge** hostile units and environmental hazards.
4. Collect **energy** dropped by enemies to choose simple weapon or ship upgrades.
5. Survive escalating waves and defeat a final powerful enemy.
6. See the team's result, then replay with different upgrades.

**Initial target:** 2–4 cooperating players; approximately 10–20 minutes per run. These values are proposals, not final decisions.

## 4. Core mechanics (proposed)

- **Controls:** responsive top-down movement, mouse aiming, primary fire and a short dash.
- **Combat:** a small set of recognizable enemy behaviors and clear damage feedback.
- **Energy/upgrades:** between waves, select from a few simple modifiers (faster fire, split shot, defensive shield, etc.).
- **Cooperation:** shared encounters, reviving a downed teammate, and a possible *electrical chain-reaction* mechanic that rewards focusing targets together.
- **World:** one compact sector / arena in the first version. Expand to procedural chunks only when the core combat is proven fun.

## 5. Multiplayer and launcher (required direction)

- A **launcher application** displays hosted games with name, player count and join status.
- Players can **host a game**, **join from the list**, or optionally connect directly.
- Each session has a **host-authoritative game simulation**; clients send their actions and receive world state through Sparkle networking.
- Games can be private/unlisted or publicly discoverable. The details of internet discovery and hosting are not yet decided.
- A disconnected player's behavior, persistence, authentication and advanced matchmaking are **not yet specified**.

## 6. First playable scope

**Include:** launcher host/join workflow, one map, two connected players, synchronized movement and shots, one enemy type, damage/health, one collectible or upgrade, and a win/lose condition.

**Exclude for now:** persistent MMO world, multiple biomes, crafting, deep inventory, character classes, account system, large procedural universes, extensive progression and competitive balance.

## 7. Decisions to validate with the designer

1. **Fundamental structure:** short co-op runs (this draft) **or** an ongoing shared world to explore?
2. **Combat identity:** straightforward space shooting **or** electron/electrical interactions as the central mechanic?
3. **Hosting:** peer-hosted sessions, standalone dedicated servers, or both?

The above concepts are **suggestions to review**, not features inherited from the old codebase or approved implementation requirements.
