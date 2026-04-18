# Warm Iverson: AI-Agent-Plays-Minecraft Framework Survey (2026-04-18)

_Full research report. Summary folded into `../warm-iverson.md` Workstream D._

## Framework Summaries

### Mindcraft / Mindcraft-CE (LLM-as-player)
Mindcraft is the flagship open-source "LLM drives a Mineflayer body" framework. As of v0.1.4 (March 2026) it supports MC **up to 1.21.11 (recommended 1.21.6)**, MIT-licensed, actively developed, with **16+ provider adapters** (OpenAI, Anthropic, Gemini, DeepSeek, Ollama, vLLM, Cerebras, OpenRouter, etc.). It exposes ~47 high-level parameterized verbs (`!goToPlayer`, `!collectBlocks`, `!newAction` with generated JS, `!givePlayer`, combat via Mineflayer-pvp), includes **vision via multimodal models**, and natively supports multi-agent via `--profiles a.json b.json`. Mindcraft-CE is an active community fork with Docker images and more experimental models. No Create-mod awareness out of the box but generic block-place/interact works because it runs through Mineflayer.

### Voyager (MineDojo)
MIT-licensed academic agent, **pinned to MC 1.19 / Fabric 0.14.18**, last meaningful commit late 2023. **OpenAI GPT-4-coupled** (hard to swap cleanly), single-agent, skill-library code generation. Historically important for the curriculum/skill-library pattern, but for a shipping product in 2026 it is effectively a reference design, not a deployment target. Co-Voyager (June 2025, external fork) extends to multi-agent but still on old MC.

### Altera / Project Sid (PIANO)
The 1000-agent civilization demo. The `altera-al/project-sid` repo contains the **technical report only — no runnable source**, no license, no Minecraft version spec. Altera AI was largely wound down in 2025 (founders/engineers dispersed). **Not viable as a framework** for Warm Iverson; only as inspiration for the PIANO pattern (parallel cognition modules, memory pipes).

### Mineflayer + plugins
The substrate everyone else uses. JS/TS, MIT, extremely mature, supports **MC 1.8 through 1.21.11**, single or multi-bot by just spawning more Node processes. Ecosystem:
- `mineflayer-pathfinder` — A* navigation, verb-level movement.
- `mineflayer-statemachine` — behavior-tree layer on top of pathfinder.
- `mineflayer-pvp`, `mineflayer-collectblock`, `mineflayer-auto-eat`, `mineflayer-tool` — drop-in verbs for combat/gather/craft.

Provider-agnostic. Zero Create-mod awareness (interacts with Create blocks only as generic blocks — levers, deployers, wrench usage not reliable).

### Claude / MCP Mineflayer wrappers (2025–2026)
Several MCP servers shipped in the last 12 months:
- **yuniko-software/minecraft-mcp-server** — Apache-2.0, 1.21.11, 20+ tools (move, fly, inventory, place/dig, furnace, entities, chat), primarily demoed with Claude Desktop but MCP-spec-compliant so any MCP client works.
- **haksndot/haksnbot-tools** — 40+ Mineflayer verbs surfaced as MCP tools, explicitly built for Claude Code.
- **hibukki** fork — tuned timeouts, crafting-distance checks.
- **leo4life2 minecraft-bot-control** — PulseMCP-listed, multi-bot oriented.

None advertise multi-agent orchestration beyond "spawn another server instance." All inherit Mineflayer's Create-mod blindness.

### MineDojo
Still receiving issues in 2026, but upstream is effectively frozen on old MC (1.11.2/1.16 via Malmo fork). It is a **simulator/benchmark**, not a production agent runtime. Skip for shipping; revisit only if you want training data.

### Baritone
The gold-standard low-level verb layer. **Forge/Fabric, up to 1.21.x (community ports)**, LGPL-3.0, maintained by cabaletta. Does `#goto`, `#mine`, `#build <schematic>`, `#farm`, `#follow` deterministically without an LLM. Single-client-bound (one Baritone per MC client), but you can run N headless clients. **Best verb-layer for "actually navigate and build reliably"** — pairs beautifully with an LLM that emits Baritone commands.

### NPC-body mods
- **Minecolonies** (ldtteam, 1.20.1 + 1.21.1, GPL-3.0) — mature long-horizon colony simulation; NPCs have fixed professions. No LLM hooks, but datapack/event hooks exist.
- **Villager Recruits** + Recruits-Extras (Forge/NeoForge, 1.20.x/1.21.x) — recruit, command, form squads, diplomacy. No LLM hooks.
- **Guard Villagers** (1.21-compatible) — simple guards, no scripting surface.
- **CustomNPCs / CustomNPCs-Unofficial** — full scripting (JS/Python), quests, dialogue trees. LLM integration possible via external HTTP from scripts but nothing turnkey.
- **LLMCraft** (Spigot/Paper, 1.12–1.20.6, Citizens-based) — drop-in AI dialogue for NPCs.
- **AI Player** (shasankp000, Fabric 1.21.1, moving to 1.21.6) — "second player" mod, Ollama + OpenAI-compatible APIs, NLP+RAG, high-level instruction decomposition. Closest single-mod hybrid to what you want.
- **Villager AI**, **CreatureChat**, **The Arbiter**, **SecondBrain**, **Steve AI** — mostly dialogue/personality layers, not embodied actors.

## 1. Capability Matrix

| Framework | Loader / MC | Move | Mine | Place | Craft | Combat | Chat | Vision | Create-aware | Multi-agent | Provider | License | Status |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Mindcraft / CE | External (Mineflayer) / 1.21.11 | Y | Y | Y | Y | Y | Y | Y (MM) | Partial (generic blocks) | Native (`--profiles`) | Agnostic (16+) | MIT | Active 2026 |
| Voyager | Fabric 1.19 | Y | Y | Y | Y | Limited | Y | N | N | N | GPT-4 coupled | MIT | Stale 2023 |
| Project Sid / PIANO | n/a | — | — | — | — | — | — | No | Yes (1000s) | n/a | No code | None | Paper only |
| Mineflayer stack | External / 1.8–1.21.11 | Y | Y | Y | Y | Y | Y | Via plugin | Generic only | Per-process | Agnostic | MIT | Active |
| Claude/MCP MF wrappers | External / 1.21.4–1.21.11 | Y | Y | Y | Partial | Partial | Y | Partial | Generic | Per-instance | Claude-primary, MCP-generic | Apache-2.0 / MIT | Active 2025–26 |
| MineDojo | Malmo / 1.11–1.16 | Y | Y | Y | Y | Y | Y | Y | N | Sim-only | Custom | MIT | Frozen |
| Baritone | Forge/Fabric / 1.21 | Y++ | Y++ | Y (schematic) | Partial | Limited | Chat-cmd | N | Generic | 1 per client | N/A (no LLM) | LGPL-3.0 | Active |
| Minecolonies | Forge/NeoForge/Fabric / 1.20.1, 1.21.1 | NPC | NPC | NPC | NPC | NPC | Limited | N | N | N (colony only) | N/A | GPL-3.0 | Very active |
| Recruits | NeoForge / 1.21.x | NPC | N | N | N | NPC++ | Cmd | N | N | Squad | N/A | ARR | Active |
| CustomNPCs | Forge / 1.20, 1.21 unofficial | Scripted | Scripted | Scripted | Scripted | Scripted | Y | N | Via script | Many | N/A, scriptable | MIT | Semi-active |
| LLMCraft | Spigot / 1.12–1.20.6 | Dialogue-only | N | N | N | N | Y | N | N | Many NPCs | Any LLM | Spigot | Active |
| AI Player mod | Fabric / 1.21.1→1.21.6 | Y | Y | Y | Y | Y | Y | N | Generic | Multiple instances | Ollama + OpenAI-API | MIT | Active |
| This repo (Gemini AI Companion) | Fabric 1.21.1 | Helper-only (via player) | Build planner | Y (player-mediated) | Y | N | Voice/chat | Live screenshots | Unclear | Single | Gemini + MCP | MIT | v1.3.2 |

## 2. Embodiment-mix Recommendation for Warm Iverson

Given solo dev, 2–4 month window, DGX Spark local inference, Create-themed SMP, 10 players:

1. **Peer bot-players (full range):** Run **Mindcraft-CE** with **local Ollama/vLLM** on the DGX (Qwen2.5-72B or Llama-3.3-70B for main loop, Gemini 2.5 Pro or Claude as fallback for hard planning). Point it at your 1.21.1 server. Mindcraft-CE's vision + Mineflayer verbs + MIT license + multi-profile spawning is the shortest path. Budget 2–3 peer bots concurrent.
2. **Deterministic verb floor:** Under the LLM, expose **Baritone** as a tool ("baritone:`#goto`/`#mine`/`#build`") by running each peer bot as a headless Fabric client with Baritone + a small bridge mod. This is what rescues build/travel reliability for Create contraptions.
3. **Helper/overseer (disembodied):** Keep **this repo's Gemini AI Companion** as-is. It already covers voice, vision, build preview, MCP, permissions — reskin as the "Warm Iverson" narrator/architect.
4. **Long-horizon colony work:** Drop in **Minecolonies** (1.21.1 matches your server) for background economy/village NPCs. Wire an LLM-commentary layer on top via its event bus — not full control, just flavor narration from your helper.
5. **Combat squads / escort NPCs:** **Villager Recruits** for commanded combat parties the peer bots or the helper can recruit and order through chat commands. Cheap, mature.
6. **Optional:** a single **CustomNPCs** installation for scripted quest-giver bodies whose dialogue is fed by your helper's LLM via HTTP.

This covers peer, helper, and NPC-body-with-LLM-brain roles without writing an agent framework from scratch.

## 3. Biggest Gaps (what you'd have to build)

- **Create-mod verb library.** Nothing understands wrenches, rotation direction, blaze burners, deployers, trains, encased chains, schedule blocks. You will need to author a Mineflayer plugin (or Baritone process extension) that exposes `rotateShaft(pos, dir)`, `setScheduleStation(...)`, `placeDeployer(...)`, etc., as first-class tools. This is the single most important build.
- **Multi-agent world-state blackboard.** Mindcraft does pairwise chat; Project Sid's PIANO is paper-only. You need a shared memory store (Redis/SQLite) and role arbitration so bots don't all mine the same chunk.
- **Persona/role supervisor.** Which body gets which model, which memory scope, which permissions. Neither Mindcraft nor this repo has a clean "cast list" config.
- **Anti-grief/guardrails for peer bots on a 10-player SMP.** Quotas, rate-limits on block changes, undo-on-demand that survives restart.
- **Bridging helper <-> peer bot.** The Fabric companion mod and the external Mindcraft Node process need a shared channel (extend your existing MCP sidecar).
- **DGX Spark model loadout.** 128 GB unified is enough for one 70B + one 13B concurrently; throughput for 3 bots each emitting 5–15 tokens/sec needs batching (vLLM or TGI) — not a framework choice, but an infra task.

## 4. Open Questions

1. Is the DGX Spark always-on for this server, or shared? Determines whether Claude/Gemini is primary and local is fallback, or vice versa.
2. How deep must Create-contraption interaction go — "operate existing player-built contraptions" vs "design and build new ones from parts"? The latter is ~4× the verb work.
3. Do peer bots need to pass as human to visitors, or is it fine that they announce as AI?
4. Is NeoForge acceptable, or strictly Fabric 1.21.1? Recruits and Minecolonies' latest 1.21 branches lean NeoForge; this may force a loader decision.
5. Voice output for peer bots (not just the helper) — required or nice-to-have?
6. Persistence horizon — should a peer bot remember last week's project across restarts? Drives memory architecture (vector DB vs session-only).
7. Headcount target: 2–3 peer bots or 8–10 (Altera-scale)? Concurrent-bot count is the single biggest cost driver.

## Relevant Files in This Repo

- `/home/user/AIminecraftBOT/README.md` — existing Gemini AI Companion v1.3.2 (Fabric 1.21.1, MCP, voice, vision).
- `/home/user/AIminecraftBOT/mcp-sidecar/` — existing MCP bridge, natural extension point for plugging in Mindcraft or Baritone bridges.

## Sources

- [Mindcraft (mindcraft-bots)](https://github.com/mindcraft-bots/mindcraft)
- [Mindcraft-CE fork](https://github.com/mindcraft-ce/mindcraft-ce)
- [Collaborating Action by Action (Mindcraft paper, 2025)](https://arxiv.org/html/2504.17950v1)
- [Voyager site](https://voyager.minedojo.org/)
- [Voyager GitHub](https://github.com/MineDojo/Voyager)
- [Co-Voyager (2025 multi-agent fork)](https://github.com/Itakello/Co-voyager)
- [Project Sid paper (arXiv 2411.00114)](https://arxiv.org/abs/2411.00114)
- [Project Sid repo (no code)](https://github.com/altera-al/project-sid)
- [Fundamental Research Labs write-up of Sid](https://fundamentalresearchlabs.com/blog/project-sid)
- [Mineflayer](https://github.com/PrismarineJS/mineflayer)
- [mineflayer-pathfinder](https://github.com/PrismarineJS/mineflayer-pathfinder)
- [mineflayer-statemachine](https://github.com/PrismarineJS/mineflayer-statemachine)
- [yuniko-software Minecraft MCP server](https://github.com/yuniko-software/minecraft-mcp-server)
- [haksndot/haksnbot-tools (Claude MCP)](https://github.com/haksndot/haksnbot-tools)
- [Minecraft Bot Control MCP (leo4life2 / PulseMCP)](https://www.pulsemcp.com/servers/gerred-minecraft-bot-control)
- [Claude Agent SDK MCP docs](https://platform.claude.com/docs/en/agent-sdk/mcp)
- [Baritone](https://github.com/cabaletta/baritone)
- [MineDojo](https://github.com/MineDojo/MineDojo)
- [Minecolonies](https://github.com/ldtteam/minecolonies)
- [Villager Recruits](https://www.curseforge.com/minecraft/mc-mods/recruits)
- [CustomNPCs-Unofficial](https://www.curseforge.com/minecraft/mc-mods/customnpcs-unofficial)
- [LLMCraft (Spigot)](https://www.spigotmc.org/resources/%E2%9C%A8-llmcraft-%E2%9C%A8-1-12-1-20-6-make-ai-powered-npcs-talk.117211/)
- [AI Player mod](https://modrinth.com/mod/ai-player)
- [AI Player GitHub](https://github.com/shasankp000/AI-Player)
- [MineLand multi-agent simulator](https://arxiv.org/html/2403.19267v1)
- [Create mod](https://www.curseforge.com/minecraft/mc-mods/create)
