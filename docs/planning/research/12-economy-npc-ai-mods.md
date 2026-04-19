# Create Remastered — Monopoly-Stack Economy + NPC + AI Research (April 2026, NeoForge 1.21.1)

_Layer-2 re-research (R5) run 2026-04-18 against the monopoly-style capitalist-empire pivot. Session archive — distilled from agent output before context scrolled. Summary folded into `../warm-iverson.md` Workstreams A + D + G._

## 1. Per-category capability matrix

### A. NPC-civilization / AI-colony mods (NF 1.21.1)

| Mod | NF 1.21.1 status | Economy depth | Verdict |
|---|---|---|---|
| **Minecolonies** | `1.1.1285` (Mar 2026), snapshot `1.1.1300` (Apr 12 2026) | Deep — 25+ professions, University research tree, Tavern visitor trading, Warehouse + Courier logistics, alliance/feud toggle | **Keep — empire engine** |
| **Minecolonies: War 'N Taxes** | `v2.2` on 1.21.1 | Per-building tax + maintenance, tax-revenue cap, tax-freeze on war outcomes, PvP arena/raid wiring, peace negotiations, SDMShop currency hook | **Keep — empire layer** |
| **MCA Reborn** | `7.7.6` NF 1.21.1 (Mar 2026) | Shallow — villager trades, relationships, simulated labor; no market/demand model | Optional flavor layer |
| **Ancient Warfare 3** | Partial 1.21.1 NF, test-build quality | Historically strong, currently unstable | **Skip** |
| **Millénaire** | Community rewrite only (`gblfxt/Millenaire-rewrite-1.21.1`), source-only on GitHub | 9 cultures, foreign-village trade | **Defer** to v0 (C5.1 decision) |
| **Custom NPCs / Humans+** | Scattered 1.21.x; Humans+ effectively dead | Dialog/quests only | Use Custom NPCs only with KubeJS scripting |
| **CreatureChat** | Yes, up to 1.21.x | LLM chat layer, no economic depth | Optional merchant-flavor layer |
| **Player2 AI NPC** | NF 1.21.1 `1.0.7` | LLM-driven NPCs | Drop-in chat-driven merchant alternative |
| **The Arbiter** | 1.21.1 with bugs | Narrative Ollama judge | Decorative |
| **SecondBrain** | Fabric-only | LLM-piloted player-like NPC | **Skip** (Fabric only) |

### B. Purpose-built economy / commerce / banking mods (NF 1.21.1)

| Mod | NF 1.21.1 | What it gives | Verdict |
|---|---|---|---|
| **Create: Numismatics** | `1.0.19` (requires Create 6.0.7) | 6 coin tiers, **Bank accounts via Bank Cards**, Vendors, Depositors w/ redstone pulse on deposit | **Keep — currency cornerstone** |
| **Numismatic Overhaul: Reforged Again** | `2.0.1` (Jul 2025) | Terraria-style purse + rebalanced villager trades | Optional — pick one or the other w/ Create: Numismatics |
| **SDMShop** | NF 1.21.1 supported | Admin shop with config-defined prices, currency integration, War 'N Taxes hook | **Keep — admin-shop + price anchor** (added explicitly during walkthrough) |
| **Stock Market (kroia)** | `1.3.1` (Feb 2025) | Global market, per-item prices | **Keep — market signal** |
| **Auction House Plus** | `1.2.3` (May 2025) | Player-to-player auctions | **Keep — instrument exchange** |
| **Real Economy** | `0.1.1+1.21.1` | Server-side multi-currency + offline payments (LuckPerms dep) | Skip — Numismatics covers this niche |
| **FTB Chunks (NeoForge)** | `2101.1.14` (Feb 2026) | Chunk claims, minimap, force-load, team ACL | **Keep — parcel/deed layer** |
| **OpenPartiesAndClaims** | Yes on 1.21 | Parties + lighter claims | Skip (FTB Chunks chosen, D.7) |
| **Szura's EconomY (SEM) 3.0** | NF 1.21.1 `6373186` | Currency wrapper + Investments tab | Optional — passive returns |
| **EconomyCraft / Magic Coins / Coins JE / Mogrul** | NF 1.21.1 | Lightweight currency wrappers | Skip (Numismatics chosen) |

**No native bonds/shares/loans mod exists for NF 1.21.x** — KubeJS gap (or skip per C5.6).

### C. Create-ecosystem economy addons (NF 1.21.1)

| Mod | NF 1.21.1 | Role | Verdict |
|---|---|---|---|
| **Create: Numismatics** | `1.0.19` | Currency + banking | **Keep** (already in B above) |
| **Create: The Factory Must Grow** | `1.2.0-1.21.1` (Jan 2026) | Diesel/oil/electricity supply chains | **Keep — vertical integration** |
| **Create Crafts & Additions** | `1.5.8` | Bridges FE↔kinetic | **Keep — RF/kinetic interop** |
| **Create Railways Navigator** | `0.8.4 / 0.9.0-C6+2` (Mar 2026) | Passenger/freight routing UI | **Keep — railroad baron meta** |
| **Create Aeronautics** | `1.0.2` (Apr 17 2026) | Airship freight | **Keep — pack identity** |
| **Create: Additional Logistics / Meta Logistics** | NF 1.21.1 | Crate routing, manifests | **Keep — logistics depth** |
| **Create: New Age** | NF 1.21.1 | Electricity/late-game progression | Optional |
| **Create: Connected** | No NF 1.21.1 release found | Cross-mod compat bridge | **Skip — never confirmed available** |

### D. Rival-merchant AI / market-reactive NPCs

**No mod in April 2026 autonomously prices goods against market state.** Closest: CreatureChat / Player2 AI NPC (chat flavor) and Minecolonies Tavern visitor trades. **KubeJS gap** to fill.

### E. AI-Player framework comparison

| Axis | AI-Player mod | Mindcraft-CE |
|---|---|---|
| NF 1.21.1 server-side bot | In-world "second player" via Ollama/API | Mineflayer bots (MC up to 1.21.11; 1.21.6 recommended) |
| Long-horizon goals | Limited | Explicit task/goal system, scenario scripts |
| Persistent memory | Minimal | **LanceDB RAG**, memory consolidation ~every 15 steps |
| Multi-bot coordination/competition | Weak | First-class: zero-sum + collab modes, extensible scoring |
| Minecolonies event hooks | None | None natively — script via Mineflayer |
| Economic-reasoning prompts | N/A | Natural-language plan/offer/status protocol |

**Pick: Mindcraft-CE.** Wins on long-horizon, RAG memory, and zero-sum multi-agent — all critical for the empire-race scenario.

## 2. Locked monopoly-style stack (post-walkthrough)

1. **Minecolonies** + **War 'N Taxes** — empire engine + tax/maintenance/peace/war
2. **Create: Numismatics** — currency + bank-account primitive
3. **SDMShop** — admin shop + price anchor + currency sink/faucet
4. **Create: The Factory Must Grow** + **Crafts & Additions** + **Railways Navigator** + **Additional Logistics** — supply chains
5. **Create Aeronautics** — airship freight (pack identity)
6. **Stock Market (kroia)** — global price feed
7. **Auction House Plus** — player-to-player trades
8. **FTB Chunks (NF)** + **FTB Teams** — parcels + ACL
9. **KubeJS** + **KubeJS Custom Events** — required glue
10. **CreatureChat** or **Player2 AI NPC** — merchant LLM flavor (optional, decide in v0)
11. **AllTheModium** — late-game material + The Other + The Beyond dimensions (F2)
12. **Mindcraft-CE** (Node, external) — Joe AI primary framework

Optional Day+1: **Millénaire community rewrite** (cultural variety), **MCA Reborn** (population realism), **Szura's EconomY 3.0 Investments tab** (passive returns).

Skip: **Ancient Warfare 3**, **OpenPartiesAndClaims**, **Real Economy**, **Create: Connected**, **SecondBrain**, lightweight currency wrappers (EconomyCraft / Magic Coins / Mogrul).

## 3. KubeJS gaps to fill (Workstream B + G work queue)

| Gap | Status |
|---|---|
| Chunk-claim purchase → Numismatics coin cost | **Required** (D.7) |
| Recurring rent/tax on held claims | Optional Day+1 |
| Tier pricing per region | Optional Day+1 |
| Claim market (buy/sell between empires) | Optional Day+1 |
| Share ledger / corporations | **Skipped** (C5.6 = role-play only) |
| Bonds / loans with interest | **Skipped** (C5.6) |
| Sealed-bid auctions / scheduled trade rounds | Optional Day+1 |
| Monopoly-board property-group set-bonuses | **Skipped** (C5.2 sandbox, no win condition) |
| Market-reactive NPC prices (Stock Market feed → Vendor prices) | Optional v1 |
| Inter-colony caravan goods transfer | Optional Day+1 |
| Win-condition / wealth leaderboard | **Skipped** (C5.2) |
| Quest custom tasks (FTB Quests XMod) | **Required** (D.10 questbook) |
| Joe AI journal entries (per-day summary) | **Required** (D.6 memory) |
| ATM Vibranium / Piglich → Create recipe integration | **Required** (F2) |

## 4. AI-player framework decision

**Mindcraft-CE** (Node) as the Joe AI primary runtime. Ambient-merchant LLM flavor (CreatureChat or Player2 AI NPC) as supplemental NPC layer if needed.

## 5. R5's 8 open questions — resolved during walkthrough

1. ✓ **Millénaire**: defer to v0 (C5.1 = c)
2. ✓ **Win condition**: open-ended sandbox, no formal end (C5.2 = e)
3. ✓ **PvP**: consensual only via Minecolonies alliance + War 'N Taxes (C5.3 = c)
4. ✓ **LLM cost**: local primary + frontier fallback bursts (C5.4 = b)
5. ✓ **Freight**: Create-native scheduling, Joe AI dispatches not pilots (C5.5 = d)
6. ✓ **Share ledger**: pure role-play + Numismatics, no custom mechanics (C5.6 = c)
7. ✓ **Parcels**: FTB Chunks + FTB Teams (C5.7 = a)
8. ✓ **Create: Connected**: drop from stack (was R5's agenda, never user-requested)

## Sources

- [MineColonies — CurseForge files](https://www.curseforge.com/minecraft/mc-mods/minecolonies/files/all)
- [Minecolonies: War 'N Taxes](https://www.curseforge.com/minecraft/mc-mods/minecolonies-war-n-taxes)
- [War 'N Taxes Addon — Modrinth](https://modrinth.com/mod/minecolony-tax-addon)
- [War-N-Taxes GitHub](https://github.com/mchivelli/War-N-Taxes-Mod-Minecolonies-Addon)
- [MineColonies About](https://minecolonies.com/about/)
- [MCA Reborn 7.7.5](https://www.curseforge.com/minecraft/mc-mods/minecraft-comes-alive-reborn/files/7278618)
- [Ancient Warfare 3 NPCs — Modrinth](https://modrinth.com/mod/ancient-warfare-3-npcs/versions)
- [Millénaire-rewrite-1.21.1 — GitHub](https://github.com/gblfxt/Millenaire-rewrite-1.21.1)
- [Create: Numismatics — CurseForge](https://www.curseforge.com/minecraft/mc-mods/numismatics)
- [Numismatics 1.0.19 NeoForge 1.21.1](https://modrinth.com/mod/numismatics/version/1.0.19+neoforge-mc1.21.1)
- [Numismatic Overhaul: Reforged Again 2.0.1](https://www.curseforge.com/minecraft/mc-mods/numismatic-overhaul-reforged-again/files/6801648)
- [Stock Market 1.3.1 NeoForge](https://www.curseforge.com/minecraft/mc-mods/stockmarket/files/6200698)
- [Auction House Plus 1.2.3 NeoForge 1.21.1](https://modrinth.com/mod/auction-house-plus/version/1.2.3+1.21.1-neoforge)
- [Real Economy 0.1.0 NeoForge 1.21.1](https://modrinth.com/mod/realeconomy/version/0.1.0+1.21.1-neoforge)
- [Szura's EconomY Mod 3.0 NeoForge 1.21.1](https://www.curseforge.com/minecraft/mc-mods/sem/files/6373186)
- [FTB Chunks (NeoForge) 2101.1.14](https://www.curseforge.com/minecraft/mc-mods/ftb-chunks-forge/files/7608681)
- [Create: The Factory Must Grow](https://www.curseforge.com/minecraft/mc-mods/create-industry)
- [Create Crafts & Additions 1.5.8](https://www.curseforge.com/minecraft/mc-mods/createaddition/files/7179893)
- [Create Railways Navigator 0.8.4](https://www.curseforge.com/minecraft/mc-mods/create-railways-navigator/files/6693815)
- [Create Aeronautics 1.0.2 1.21.1](https://modrinth.com/mod/create-aeronautics/version/HY8u0JqC)
- [Create: Additional Logistics](https://www.curseforge.com/minecraft/mc-mods/create-additional-logistics)
- [Create: Meta Logistics](https://modrinth.com/mod/create-meta-logistics)
- [Mindcraft-CE GitHub](https://github.com/mindcraft-ce/mindcraft-ce)
- [Mindcraft-CE site](https://mindcraft-ce.com/)
- [AI-Player — CurseForge](https://www.curseforge.com/minecraft/mc-mods/ai-player)
- [Player2 AI NPC NeoForge 1.21.1](https://modrinth.com/mod/player2npc/version/IlbV2qGH)
- [CreatureChat — CurseForge](https://www.curseforge.com/minecraft/mc-mods/creaturechat)
- [The Arbiter — Modrinth](https://modrinth.com/mod/the-arbiter)
- [KubeJS Custom Events — CurseForge](https://www.curseforge.com/minecraft/mc-mods/kubejs-custom-events)
