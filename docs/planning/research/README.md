# Create Remastered — Layer-2 Research Reports

> **Naming note:** these reports were written under the working title **"Warm Iverson"**
> on 2026-04-18. The product was renamed to **Create Remastered** (with AI layer
> **Joe AI**) later the same day. Reports are preserved as dated archives — read
> "Warm Iverson" throughout as "Create Remastered."

Six parallel research sweeps run on 2026-04-18 as input to the Create Remastered plan
(`../warm-iverson.md`). Each file below is the full, untrimmed report produced by
the subagent for its topic, preserving every URL citation and open question.

| # | File | Feeds Workstream | Headline |
|---|---|---|---|
| 01 | [loader-mc-version.md](01-loader-mc-version.md) | A | **NeoForge 1.21.1** primary (~80% conf), Forge 1.20.1 runner-up. Fabric 1.21 blocked by Create's Fabric port frozen on 1.20.1. |
| 02 | [ai-agent-landscape.md](02-ai-agent-landscape.md) | D | **Mindcraft-CE + Baritone** for peer bots, existing Gemini mod as helper, Minecolonies + Recruits for NPC bodies. |
| 03 | [quest-framework.md](03-quest-framework.md) | B | **FTB Quests + XMod Compat + Optimizer + Freeze Fix**. Questify runner-up. Reject BetterQuesting and Heracles. |
| 04 | [texture-generation.md](04-texture-generation.md) | B | **SDXL + Pixel Art XL LoRA + custom LoRA + ControlNet-Depth on ComfyUI** (local on DGX Spark). |
| 05 | [self-healing-ops.md](05-self-healing-ops.md) | C | Pterodactyl + Spark + SparkWebAPI + FTB Backups 2 + LXC watchdog sidecar + bespoke diagnostic agent. |
| 06 | [control-plane-refs.md](06-control-plane-refs.md) | J | Next.js 16 + shadcn + LangGraph + Redis Streams + Auth.js + discord.js + BlueMap + iframe Pterodactyl. |

## Status after assumption walk (2026-04-18)

After ratifying assumptions for the loader decision, these reports have deltas:

| # | Status | Delta |
|---|---|---|
| 01 | Standing | Sinytra Connector added to server stack — not evaluated in original report; small ops complexity on top of NF 1.21.1 pick. |
| 02 | **Materially changed** | Helper role is no longer "reskin existing Gemini mod" — user dropped the existing Fabric mod from production (path B). Helper becomes a **greenfield external build** (Node/Python + Mineflayer + MCP + DGX Spark). Recruits → Guard Villagers. See re-research 07/09/10. |
| 03 | Standing | Loader pick confirms NF 1.21.x/1.20.1 assumption. |
| 04 | Standing | — |
| 05 | Minor | Config-drift hashing must include Sinytra + Fabric-mod configs under Sinytra. |
| 06 | Minor | MCP-over-RCON is now the only MC-integration path; no parallel existing-mod MCP channel. Rec still valid. |

### Re-research sweep reports (2026-04-18, all complete)

| # | File | Feeds | Headline |
|---|---|---|---|
| 07 | [greenfield-helper-arch.md](07-greenfield-helper-arch.md) | D+E | **Litematica/Syncmatica + BlueMap + prismarine-viewer + discord.js voice + faster-whisper + piper1-gpl**. One ~200-line server-side-only SVC bridge jar required if in-world voice is in scope. |
| 08 | [nf-sinytra-mineflayer-compat.md](08-nf-sinytra-mineflayer-compat.md) | A+D | **Conditionally viable**. Sinytra 2.0.0-beta.14. **Critical: Lithium via Sinytra NOT supported — use Radium Reforged (NF-native)**. Mineflayer needs `physicsEnabled:false` workaround. Anticheat mods blacklisted. |
| 09 | [guard-villagers-depth.md](09-guard-villagers-depth.md) | A+D | Guard Villagers 2.4.7 (Mar 2026) on NF 1.21.1 is active. Covers hiring + combat + defense but no squad UI / formations / ranks / banners — Joe AI fills those via peer bots + KubeJS. |
| 10 | [existing-mod-port-audit.md](10-existing-mod-port-audit.md) | E | Top 5 port priorities: Build Planner, MCP Bridge surface, Batch executor, Undo, Vision. Retire: highlights (→ Litematica), Fabric wiring, in-game setup wizard (→ web app). |

Each report ends with **open questions** — those are queued for the per-workstream Layer-2 deep-dive sessions.

See also [ASSUMPTIONS.md](ASSUMPTIONS.md) — the full catalogue of assumptions baked into these reports (user-stated facts, Layer-1 derivations, parent-agent prompt context, subjective agent judgment calls, silent inferences, methodology choices, and deliberate exclusions).
