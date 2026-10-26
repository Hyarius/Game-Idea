# Vaylora — Game Design Document

**Status:** Consolidated working design (approved decisions through VAY-052)  
**Updated:** 26 October 2026  
**Genre:** Single-player creature-collection tactical RPG  
**Target:** PC; Sparkle engine

> This is a player-facing game design, not a technical specification or an implementation plan. Only decisions already approved for Vaylora are treated as requirements. Numbers, content and mechanics not yet discussed remain open.

## 1. Game vision

Vaylora combines Pokémon-inspired creature discovery and collection with Dofus-inspired grid-based tactical battles. **Exploration and collection are equally important.** The player explores a finite procedural fantasy world, captures wild creatures by fulfilling tactical conditions, develops individual creatures through branching Feat Boards, defeats eight arena champions in any order, and ultimately challenges an elite League.

**Core loop:** Explore a region → encounter creatures and trainers → fight and capture → develop the squad → prepare in town → tackle new challenges and arenas.

Creatures are **combat companions only**: they have no overworld traversal, gathering or environmental powers. The player character does not participate directly as a combat unit. (VAY-001–003, VAY-006)

## 2. Exploration and procedural world

- Each playthrough has a **finite world composed of interconnected, independently generated regions**. Regions persist and can be revisited; different playthroughs can generate different worlds. This is **not** one seamless open world. (VAY-004–005)
- Regions can contain varied biomes, towns, routes, challenges, wild species and trainers.
- Both exploration and battle use the **same tile-based world**. The explorer moves tile by tile. Wild encounters occur **randomly** during exploration; wild creatures are not simulated as roaming overworld actors. (VAY-009, VAY-011)
- Once an encounter begins, the existing nearby tiles, terrain and obstacles **become the battlefield**. Combat is not moved to an unrelated generated or handcrafted encounter map. (VAY-010)
- Before turns begin, the player enters a **tactical deployment phase**, placing their participating creatures on permitted starting tiles. Specific deployment boundaries remain undefined. (VAY-012)

Exact region geometry, transitions, encounter frequencies and world-generation algorithms are not yet decided.

## 3. Team and creature ownership

The player's active team contains **up to six creatures**, and all six can fight simultaneously. The player **manually controls** every allied creature during battle. Duplicates are unrestricted: a team may consist of six individuals of the same species. Each individual has its own progression and permanent evolution choices. (VAY-013, VAY-029–030)

The player may own more than six creatures. Extras are held in a **PC-like reserve**. Team swaps and reserve access are available **only in towns**, not freely in the wilderness. After capturing a creature, it can enter an available active slot; if the team is full, it goes to the reserve. (VAY-037, VAY-040)

## 4. Tactical combat

### 4.1 Readiness and turn order

Instead of fixed rounds, every creature has an individual **readiness gauge**:

1. All gauges start at **zero** at the beginning of combat.
2. Each species has a base characteristic: the **simulated number of seconds needed to fill its gauge**.
3. Gauges fill while combat time progresses. When a creature becomes ready, time **pauses** and the creature takes its turn without real-time decision pressure.
4. After the turn, that creature's gauge **always resets to zero**, independent of the actions it performed; then time progression resumes.
5. An ability that fills another creature's gauge can make that creature act **immediately after the current turn**, without waiting for further simulated time. (VAY-014–016, VAY-031–035)

If several creatures become ready simultaneously, resolve ties deterministically by interleaving **player team slot 1, enemy team slot 1, player slot 2, enemy slot 2**, etc., skipping slots whose creatures are not ready.

Feat Board nodes can permanently reduce a creature's base fill duration. Combat effects can speed up, slow down or suspend filling. Abilities can add, drain, or transfer gauge progress either as **absolute simulated seconds** or as **percentages**, defined per ability. A stun **stops natural filling and prevents starting a turn**; an external ability may still fill the stunned creature's gauge, but it must wait until the stun expires before acting. (VAY-033–036)

### 4.2 Actions

Each creature separately has:
- **Action Points (AP):** spent on attacks and abilities.
- **Movement Points (MP):** spent on tile movement.

A creature can take multiple actions and move during its turn while it has sufficient AP/MP. Exact values and ability costs are not yet defined. (VAY-017)

## 5. Feat Boards: progression and evolution

**No traditional XP or creature levels.** Every **species** has a unique Feat Board describing every possible form, active ability, passive and stat bonus that individuals of that species can attain. Creatures develop by completing feats in tactical battles. (VAY-008, VAY-018)

### 5.1 Structure and specialization

- Boards have progression tiers provisionally called **circles** (the name and shape are not final).
- Every creature begins at **circle 0** with a central node that grants its **base form and one basic ability**.
- Linked nodes grant abilities, passives, stats and further choices. Distinct regions of a board can correspond to different forms.
- **Evolution nodes in the same tier are mutually exclusive.** Selecting an evolution permanently excludes other evolution nodes of that tier. Subsequent progression follows nodes linked to the chosen form.
- Evolution is **irreversible**. Two individuals of the same species can specialize differently, for example in damage, placement/crowd control or support.

### 5.2 Feats and when they count

Each node specifies its own feat conditions, including combinations of mandatory and alternative requirements. Conditions can count damage, healing, status applications, affected enemy counts and other tactical events:
- **Cumulative across battles**, or constrained to **one battle**, **one turn**, or **one action**.
- **Single-target** results or **combined multi-target** results, as specified by that node.
- Accessible nodes only: counters start **after the prerequisite parent has been unlocked**. Earlier actions do not count retroactively.
- Several independent, already-accessible nodes can progress and unlock during the same battle, but a child of a node newly unlocked in that battle **only becomes available after battle**, so it cannot progress until a later fight.
- All new node unlocks and their effects are **applied at the end of combat**, never partway through an encounter.
- Feats and unlocks are retained even when the player loses. (VAY-022–028)

### 5.3 Automatic unlocks, deliberate evolution

Ordinary ability, passive and stat nodes activate **automatically after combat** once their feats are complete. Evolutions, in contrast, **always need explicit approval**.

After combat, the game offers **every evolution option that has become available**. The player may choose one or decline all. Declined evolutions remain available for later manual activation through the creature's Feat Board outside battle. Accepting one permanently locks out its competing evolutions within that tier. (VAY-019–021)

The number of circles, their contents, exact conditions and individual species designs remain undefined.

## 6. Creature encounters and capture

Each wild species has **specific tactical capture conditions**, rather than a random capture chance or a capture item. As soon as the conditions are fulfilled, that creature **immediately exits the battle** and is marked as captured. At battle end, each captured creature is offered to the player for the active team or reserve. **Captures are kept even if the player subsequently loses.** (VAY-003, VAY-037, VAY-039)

A **bestiary/encyclopedia** progressively reveals hints about each species' capture requirements as the player encounters and battles it. The player can succeed through experimentation **before** all hints are known. (VAY-038)

## 7. Resources, health, towns and defeat

- HP and relevant status conditions **persist between battles**.
- **No items or inventory whatsoever**: no consumables, equipment, quest items or gathered resources. There are no capture or healing items.
- Creature abilities, including healing, **cannot be used outside combat**.
- **Towns** provide full team healing and access to the creature reserve.
- On defeat, the player returns to the **last visited town** with their team fully healed. Earned feats and captures are **not lost**.
- Fleeing **wild encounters** is immediate, guaranteed and available during combat. **Champion** battles cannot be escaped this way. (VAY-041–046)

This intentionally makes long-distance exploration a matter of managing persistent injuries and planning trips between towns without an inventory economy.

## 8. Arena progression, trainers and the League

### 8.1 Eight badges, unrestricted arena order

The player may challenge the **eight regional arenas in any order**. Every champion and other badge-scaled encounter has **eight authored difficulty configurations**, according to the number of badges already held:

| Badges held before encounter | Configuration |
| --- | --- |
| 0 | 1 |
| 1 | 2 |
| 2 | 3 |
| 3 | 4 |
| 4 | 5 |
| 5 | 6 |
| 6 | 7 |
| 7 | 8 |

This progression is independent of creature XP levels, which do not exist. The behavior of scalable encounters after eight badges is still unspecified. (VAY-007)

The **eight champions, their battle teams and arena layouts are handcrafted**, with eight team configurations per champion. Only arena placement within the procedural world varies between playthroughs; arena battlefields are integrated into the normal world tiles. (VAY-050)

Defeating a champion for the first time awards its badge. A defeated champion can be challenged again **after earning another badge**, using the corresponding new configuration, without awarding another copy of its badge. (VAY-051)

### 8.2 Regular trainers

There are **many visible NPC trainers** across the world. The player initiates their battles **voluntarily by interacting**; trainers never automatically challenge players on sight. Trainer difficulty is badge-scaled. Once defeated, a trainer becomes available for a rematch **after the player earns a new badge**, with the updated team configuration. (VAY-047–049)

### 8.3 Final challenge

After collecting all eight badges, the player can enter a League composed of **four elite tacticians fought successively, followed by one final champion**. These five opponents have predefined teams and distinct tactical specializations. (VAY-052)

**Healing between League fights remains undecided (VAY-053).** No option has been selected.

## 9. Scope and open details

This GDD is the **approved concept**, not a commitment to implement every feature simultaneously. It does not import MMO systems from Erelia.

Important matters left open include the detailed generation and connection of regions, placement rules, encounter rates, creature roster and Feat Boards, exact AP/MP/balance figures, the behavior of normal encounters after badge eight, detailed withdrawal rules outside wild/champion battles, League recovery, story, art direction, UI and production milestones.

Future design discussion should resolve only consequential remaining choices; previously approved rules should not be reinterpreted as proposals.
