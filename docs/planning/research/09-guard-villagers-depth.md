# Guard Villagers on NeoForge 1.21.1 — Research for Create Remastered (as Recruits substitute)

_Layer-2 re-research (R3) run 2026-04-18. Summary folded into `../warm-iverson.md` Workstream A/D._

## 1. Current 1.21.1 NeoForge build
- Latest: `guardvillagers-2.4.7-1.21.1.jar` (CurseForge, released Mar 3 2026). Previous 2.4.5 was live through Jan 2026 modpacks. Maintenance is active but low-frequency; maintainer seymourimadeit/almightytallestred ships small patches every 1–3 months. Bug tracker: github.com/seymourimadeit/guardvillagers.

## 2. Feature list (verbs)
- **Spawning**: natural spawn of 6-guard groups per village; iron sword or crossbow preset.
- **Hiring**: crouch + right-click a nitwit or unemployed villager while holding a sword/crossbow converts them into a guard.
- **Inventory access**: helmet, chest, legs, feet, mainhand, offhand, plus shield/food/potion slots. Requires high reputation or Hero of the Village.
- **Combat**: auto-targets zombies, illagers during raids, other hostiles. Shield block, sword-kick, crossbow fire, auto-eat food/drink potions from offhand at low HP.
- **Commands (GUI)**: Follow, Patrol/Stand Ground (home position), inventory. Two buttons: follow (left), patrol (right).
- **Reputation roles**: Bad Player (after 3 hits/kills on protected mobs), Normal, Hero of the Village.

## 3. KubeJS / datapack surface
- **Datapack**: `data/guardvillagers/armor_sets/*.json` (1.20.1+) and `guard_{helmet,chestplate,legs,feet,main_hand,off_hand}` loot-table overrides — fully moddable equipment pools. Example pack for 1.21.1 ships with the repo.
- **Config (TOML)**: toggle patrol AI (for lag), spawn rates, illager friendliness, raid participation, guard reputation thresholds.
- **KubeJS**: no first-party bindings. Entity is a normal `LivingEntity` subclass, so KubeJS event hooks work (`EntityEvents.spawned`, `PlayerEvents.entityInteract`, `BlockEvents.rightClicked`). Custom roles, dynamic hire prices, slot locks, and extended targeting must be written as scripts against the entity tag/NBT — no declarative role API exists.

## 4. In-game UX
Player walks up to a guard they trust → right-click → GUI opens with 6 armor/weapon slots + shield + food slot, plus Follow and Patrol buttons. No keybinds, no party list, no chat commands, no group UI — interaction is **1 player ↔ 1 guard at a time**.

## 5. Gap vs Villager Recruits
| Feature | Recruits | Guard Villagers |
|---|---|---|
| Hire w/ emeralds | Yes | Sword/crossbow barter |
| Group/squad mgmt GUI | Yes | **No** |
| Formations (line, wedge, box) | Yes | **No** |
| Ranks / promotion | Yes | **No** |
| Patrol routes (multi-waypoint) | Yes | Single home-point only |
| Retreat / rally / hold fire cmds | Yes | **No** (follow/stand only) |
| Banners, assembly points | Yes | **No** |
| Diplomacy / teams / factions | Yes | **No** (reputation only) |
| Siege weapons | Yes | **No** |
| Cross-player coordination | Yes | **No** |

## 6. Addons
- **Rally of the Guard** — scroll/Commander's Ledger opens a panel, emerald-based ownership, teleport-rally of owned guards. *Fabric-only as of 1.21.1-1.2.4 (Aug 2025)* — not usable on NeoForge. Good design reference.
- **No More Helpless Guards** (datapack) — enchants + modded weapon pools; no AI changes.
- **Recruit Guard Villagers**, **Village Guards** datapacks — spawn/equipment tweaks only.

## 7. Bugs / perf (pack-scale)
- Patrol AI is the documented lag source; disable in config for 500-mod/10-player servers.
- Historical issues: dedicated-server desync of Follow/Stay buttons (mrsterner fork #70 — upstream fix landed in 2.2.1), guard food over-consumption, no-spawn villages. No open NF 1.21.1 crash reports on 2.4.x as of April 2026.

## Capability matrix (NF 1.21.1)
| Domain | State |
|---|---|
| Hire/convert | Shipped |
| Per-guard equip GUI | Shipped |
| Follow / Stand | Shipped |
| Combat AI + shield | Shipped |
| Raid defense | Shipped |
| Squad commands | Missing |
| Scripting hooks | KubeJS-via-entity-events only |
| Datapack equipment | Strong |

## Joe AI gap-fill table
| Recruits feature | Joe AI fills via |
|---|---|
| Squad formations | Peer bots hold relative offsets |
| Ranks/roles | Agent metadata + KubeJS NBT tag |
| Multi-waypoint patrol | Helper agent issues Follow/Stand toggles |
| Retreat/rally | Agent teleport request + Follow toggle |
| Banners/rally points | Agent-placed marker block + pathing |
| Diplomacy/factions | Joe faction layer, guards inherit owner allegiance |
| Siege weapons | Out of scope — see Risks |

## KubeJS extension plan
1. **`/guard squad` chat cmds** — KubeJS registers commands that iterate trusted guards within radius and flip follow/patrol NBT.
2. **Rally beacon item** — right-click emits event; script teleports owned guards (UUID tag) to player (Rally-of-the-Guard parity on NF).
3. **Banner tag** — banners with NBT become patrol anchors; script rewrites guard home position on place.
4. **Dynamic hire cost** — intercept `PlayerEvents.entityInteract` on villagers, require emeralds, convert via NBT.
5. **Role presets** (archer / shield / medic) — on hire, apply equipment + targeting tag; targeting enforced via per-tick proximity script.

## Risks & alternates
- No native squad UI means Joe AI must own all coordination; if Joe latency is high, feel will be worse than Recruits.
- Alternate mods if Guard Villagers proves thin: **Humans+** (player-like NPCs, NF 1.21.1), **MCA Reborn** (families + some combat), **Tough As Nails** is unrelated (survival) — cut from list. **Small Ships / Create-cannons** can substitute siege.
- Fabric-only Rally of the Guard cannot be ported 1:1; reimplement in KubeJS.

## Open questions
- Does 2.4.7 expose any new events/capabilities beyond 2.4.5? (changelog not yet scraped.)
- Can KubeJS mutate `home position` NBT reliably without AI-goal reset?
- Raid-scale perf with patrol disabled and 50+ guards across 10 players — needs load test.
- Is there a NF 1.21.1 port of Rally of the Guard in progress? (only Fabric found.)

## Sources
- [Guard Villagers — CurseForge](https://www.curseforge.com/minecraft/mc-mods/guard-villagers)
- [Guard Villagers 2.4.0-1.21.1 file](https://www.curseforge.com/minecraft/mc-mods/guard-villagers/files/7259807)
- [Guard Villagers — Modrinth](https://modrinth.com/mod/guard-villagers)
- [Guard Villagers — Grokipedia](https://grokipedia.com/page/Guard_Villagers)
- [seymourimadeit/guardvillagers GitHub](https://github.com/seymourimadeit/guardvillagers)
- [Issue #186 NeoForge crash](https://github.com/seymourimadeit/guardvillagers/issues/186)
- [Issue #70 follow/stay on dedicated servers (fork)](https://github.com/mrsterner/GuardVillagers/issues/70)
- [Rally of the Guard — CurseForge](https://www.curseforge.com/minecraft/mc-mods/rally-of-the-guard-guardvillagers)
- [No More Helpless Guards datapack](https://modrinth.com/datapack/no-more-helpless-guards-(guard-villagers))
- [Recruit Guard Villagers datapack](https://modrinth.com/datapack/recruit-guard-villagers)
- [Villager Recruits — CurseForge](https://www.curseforge.com/minecraft/mc-mods/recruits)
- [Villager Recruits — Modrinth versions](https://modrinth.com/mod/villager-recruits/versions)
- [talhanation/recruits GitHub](https://github.com/talhanation/recruits)
- [KubeJS](https://kubejs.com/)
- [Minecraft Guides — Guard Villagers](https://www.minecraft-guides.com/mod/guard-villagers/)
