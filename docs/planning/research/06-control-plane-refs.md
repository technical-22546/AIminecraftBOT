# Warm Iverson Control Plane — Stack Recommendation (April 2026)

_Full research report. Summary folded into `../warm-iverson.md` Workstream J._

## 1. "Hermes" and "OpenClaude / OpenCode" — what they are, what to borrow

**Hermes Agent** (Nous Research, v0.10 shipped April 13 2026) is an open-source self-improving agent that runs as a long-lived process and exposes a local web dashboard. Its dashboard shows a session list keyed by **source platform icon** (CLI, Telegram, Discord, Slack, cron), model, message count, tool-call count, and last-active timestamp. The inspector tab drills into each session's tool calls and memory writes. Borrow: **platform-tagged session rows**, **skills browser**, **gateway/config panel replacing YAML editing**, **background-process monitor**.

**OpenCode** (sst/opencode, ~95k GitHub stars) is an AI coding agent built for the terminal but packaged as four surfaces against one backend: **TUI (Bubble Tea), VS Code extension, desktop, and web**. It introduces first-class **Agents** (Build / Plan built-ins, plus user-defined with scoped tool access). Borrow: **one gateway, many clients** architecture; **Plan vs Act split**; **custom agent YAML** with tool allowlists.

**OpenClaude** is overloaded — the most credible referent in 2026 is Gitlawb/openclaude, a multi-provider CLI derived from the leaked Claude Code source (leak by Chaofan Shou, March 31 2026). The UI pattern worth borrowing is actually **Claude Code UI / CloudCLI** (siteboon/claudecodeui) and **Claudia GUI** — session mgmt, mobile/web remote control, and a visual approval step for every tool call.

## 2. Open-source agent control-plane UIs — what to fork

| Project | Steal |
|---|---|
| **Cline** (VS Code) | Transparent per-request cost/token breakdown, **Plan/Act mode toggle**, step-by-step approval cards |
| **Aider** | Terminal simplicity as a fallback; diff-first review UX |
| **OpenHands** (v1.6 Mar 30 2026, Kubernetes + Planning Mode beta) | Sandboxed Docker loop, local React SPA + REST API separation, live event stream |
| **LangGraph Studio** | **Graph flowchart view** of agent workflow, per-node token counts, replay-with-modified-input |
| **AutoGen Studio** (now folded into MS Agent Framework 1.0 Q1 2026) | Conversation-based debugger for multi-agent chat |
| **Claudia GUI / CloudCLI** | Remote Claude-Code-session manager for mobile/web |
| **Hermes dashboard** | Platform-tagged session table, skills registry |

Common pattern to replicate: left rail = sessions; center = **tool-call timeline** with expandable args/results; right rail = **approval queue + cost meter**; bottom = streaming console.

## 3. Minecraft server dashboards

- **BlueMap** — 3D, modern, has [BlueMapAPI](https://github.com/BlueMap-Minecraft/BlueMapAPI) for programmatic markers/POIs; frontend reads live data from `/maps/<world>/live/`. Best for iframe embed + "world map post" Discord feature (server-side snapshot → BlueMap API marker → screenshot).
- **squaremap / Pl3xMap** — lightweight 2D, fast rendering. Use if BlueMap is too heavy.
- **Dynmap** — classic, feature-rich but heavier.
- **[LiveAtlas](https://github.com/JLyne/LiveAtlas)** — single frontend that unifies Dynmap/squaremap/Pl3xmap/Overviewer; worth embedding instead of native UIs.
- **Pterodactyl** (PHP/React/Go, Docker-isolated) exposes a REST API from Wings; embed console via iframe or proxy its WebSocket. **AMP** is paid but polished — consider only if multi-game.

Pattern: don't rebuild server panel. **Iframe Pterodactyl** for lifecycle + console, and **BlueMap** for map; proxy both under your gateway so Discord OAuth roles gate access.

## 4. Discord ↔ Minecraft bridges

| Bridge | Chat bridge | Console relay | Slash cmds | Per-channel |
|---|---|---|---|---|
| **DiscordIntegration (Forge)** | Yes, bidirectional | Yes | Limited | Single channel |
| **DI (Fabric/Forge mod)** | Yes + death/advancement | Yes | Basic | One |
| **MC-DC Bridge** | Client-side, bidirectional | No | Rich: `/setchannel`, `/link`, `/blacklist*`, `/pingconfig` | **Yes** |
| **DiscordSRV** (classic, Bukkit) | Yes | Via addons | Mature | Yes |
| **DiSky** (Skript) | Scriptable | Yes | Yes | Custom |

Recommendation: use **DiscordSRV** (or DI for Forge) only as the **in-game chat shim**; do NOT make it your control plane. All `/srv cmd`, approvals, and agent events should go through your own bot, talking to an **MCP-over-RCON bridge** (e.g. [Peterson047/Minecraft-MCP-Server](https://github.com/Peterson047/Minecraft-MCP-Server) or Kyle Kelley's minecraft-rcon-mcp) so the agent framework and the bot hit one API.

## 5. Real-time bus

Solo-dev recommendation: **Redis pub/sub + Streams** fronted by a **WebSocket** gateway. Redis hits p99 ~0.8 ms, is already your cache/queue/rate-limiter, and NATS JetStream (3.2 ms, clustering-first) is overkill until you scale past one box. Use **SSE** only for one-way feeds (server console tail to read-only viewers) — 30–40% lower server cost than WS for unidirectional. Keep WS for the authoring/approval channels.

## 6. Auth

**Auth.js (next-auth v5)** with the Discord provider, plus a thin **role-mapped ACL** table keyed on Discord guild-role IDs per environment (`dev`, `sandbox`, `prod`). Lucia is sunset as a library in late 2024 (now an architecture guide). **Better Auth** (18k+ stars early 2026) is the modern contender — choose it if you want 2FA/passkeys/magic-link plugins out of the box; otherwise Auth.js wins on Discord-OAuth maturity and Next.js integration.

## 7. Frontend

**Next.js 16 (App Router) + shadcn/ui + Tailwind v4 + TypeScript.** Fork **[Kiranism/next-shadcn-dashboard-starter](https://github.com/Kiranism/next-shadcn-dashboard-starter)** (Next 16, themes, Kanban, Recharts) or **[arhamkhnz/next-shadcn-admin-dashboard](https://github.com/arhamkhnz/next-shadcn-admin-dashboard)**. Remix/SvelteKit are fine but the shadcn ecosystem + Auth.js + Vercel templates make Next.js the lowest-friction solo path.

---

## Recommended stack (end-to-end)

- **Frontend**: Next.js 16 + shadcn/ui + Tailwind v4; TanStack Query; Zustand for local stores; xterm.js for consoles; iframe BlueMap + Pterodactyl.
- **Backend gateway**: Node.js (Fastify) or Python (FastAPI) — pick your agent-framework language. Expose REST + **WebSocket /events** + SSE /logs.
- **Real-time bus**: Redis (pub/sub + Streams for durable agent events).
- **Agent framework**: LangGraph (Python) with LangSmith tracing, OR the new Microsoft Agent Framework if .NET-comfortable. LangGraph Studio is your dev-time debugger.
- **MC integration**: Minecraft-MCP-Server (RCON) + DiscordSRV for in-game chat mirroring.
- **Map**: BlueMap + LiveAtlas front.
- **Server panel**: Pterodactyl (iframe + API proxy).
- **Discord bot**: **discord.js v14** (Node keeps one language with the gateway) or Pycord if Python.
- **Auth**: Auth.js v5, Discord provider, Postgres session store, role-mapped ACL.
- **DB**: Postgres + Drizzle ORM.
- **Deploy**: Docker Compose on Proxmox LXC; Traefik for TLS; one-liner bootstrap.

## Starter templates to fork

- [Kiranism/next-shadcn-dashboard-starter](https://github.com/Kiranism/next-shadcn-dashboard-starter) — dashboard skeleton.
- [sst/opencode](https://github.com/sst/opencode) — multi-client gateway pattern.
- [OpenHands/OpenHands](https://github.com/OpenHands/OpenHands) — agent loop + sandbox reference.
- [siteboon/claudecodeui](https://github.com/siteboon/claudecodeui) — remote agent session UI.
- [JLyne/LiveAtlas](https://github.com/JLyne/LiveAtlas) — unified map frontend.
- [Peterson047/Minecraft-MCP-Server](https://github.com/Peterson047/Minecraft-MCP-Server) — RCON→MCP bridge.
- [BlueMap-Minecraft/BlueMapAPI](https://github.com/BlueMap-Minecraft/BlueMapAPI) — marker API.

## Integration diagram

```
                        +-------------------+
                        |   Next.js Web     |
                        |  (shadcn admin)   |
                        +---------+---------+
                                  |  HTTPS + WS
         Discord OAuth            v
+---------------+        +-----------------------+        +------------------+
| Discord Bot   |<------>|  Control-Plane        |<------>| Agent Framework  |
| (discord.js)  |  WS    |  Gateway (Fastify)    |  gRPC  | (LangGraph +     |
|  /srv cmd     |        |  REST + WS + SSE      |        |  LangSmith)      |
+------+--------+        +----+------+-----+-----+        +--------+---------+
       |                      |      |     |                       |
       | DiscordSRV chat      |      |     |                       | GPU jobs
       v                      v      v     v                       v
+---------------+   +----------+  +-----+ +-----+          +-------------------+
| MC Server     |<--| MCP/RCON |  |Redis| | PG  |          |  DGX Spark node   |
| (Fabric+mods) |   |  Bridge  |  |pub/ | |     |          | (remote inference |
| BlueMap, Pl3x |   +----------+  |sub+ | +-----+          |  + heavy agents)  |
+------+--------+                 |strm |                  +-------------------+
       | iframe                   +-----+
       v
+---------------+      +----------------+
| BlueMap Web   |      | Pterodactyl    |
|  (:8100)      |      | Panel + Wings  |
+---------------+      +----------------+
```

## Biggest risk for a solo 2–4 month ship

**Scope creep on the authoring tools** (KubeJS recipe editor, texture-prompt designer, questbook editor). Each is a mini-IDE. Ship v1 as **read-only viewers + raw JSON/KubeJS edit + validate-on-save + server hot-reload**, not drag-and-drop GUIs. Second risk: **approval-queue semantics under disconnect** — design the agent event bus idempotent from day one (Redis Streams with consumer groups), otherwise a dropped WS = silently lost approvals.

## Open questions

1. **Agent framework language** — LangGraph/Python vs Microsoft Agent Framework/.NET vs TypeScript (OpenCode-style)? Picks your gateway runtime.
2. **Self-host LLM or API-only?** DGX Spark implies local inference — do we need vLLM/TGI in the stack diagram, or is it API-gateway-only via OpenRouter/Anthropic/OpenAI?
3. **Is Pterodactyl acceptable as the server-lifecycle UI**, or must the web app natively own start/stop/backup?
4. **Modpack distribution target** — CurseForge, Modrinth, or both? Affects the "pack creator" surface heavily.
5. **Forge, Fabric, NeoForge, or multi-loader?** Bridge-mod choice depends.
6. **Single SMP or fleet?** Per-environment channels (dev/sandbox/prod) implies ≥3 MC instances; confirm we're not building multi-tenant from day one.
7. **KubeJS editor scope** — raw editor + validator, or full visual recipe graph?

## Sources
- [Hermes Agent — Web Dashboard](https://hermes-agent.nousresearch.com/docs/user-guide/features/web-dashboard)
- [NousResearch/hermes-agent (GitHub)](https://github.com/nousresearch/hermes-agent)
- [OpenCode](https://opencode.ai/)
- [sst/opencode DeepWiki](https://deepwiki.com/sst/opencode)
- [OpenHands (GitHub)](https://github.com/OpenHands/OpenHands)
- [OpenHands review 2026](https://vibecoding.app/blog/openhands-review)
- [siteboon/claudecodeui](https://github.com/siteboon/claudecodeui)
- [Gitlawb/openclaude](https://github.com/Gitlawb/openclaude)
- [Claudia GUI](https://claudia.so)
- [cline/cline](https://github.com/cline/cline)
- [Agentic CLI Tools Compared](https://aimultiple.com/agentic-cli)
- [LangGraph vs AutoGen Studio 2026](https://myengineeringpath.dev/tools/autogen-vs-langgraph/)
- [Dynmap vs BlueMap vs Squaremap vs Pl3xMap](https://blog.berrybyte.net/dynmap-vs-bluemap-vs-squaremap-which-map-plugin-is-best-for-your-minecraft-server/)
- [LiveAtlas](https://github.com/JLyne/LiveAtlas)
- [BlueMap](https://bluemap.bluecolored.de/)
- [BlueMapAPI](https://github.com/BlueMap-Minecraft/BlueMapAPI)
- [Pterodactyl Panel](https://github.com/pterodactyl/panel)
- [MC-DC Bridge](https://modrinth.com/mod/mc-discord-bridge)
- [Discord Integration (Forge)](https://www.curseforge.com/minecraft/mc-mods/dcintegration)
- [Peterson047/Minecraft-MCP-Server](https://github.com/Peterson047/Minecraft-MCP-Server)
- [Kyle Kelley minecraft-rcon-mcp](https://skywork.ai/skypage/en/kyle-kelley-minecraft-rcon-server/1980054865061728256)
- [Real-Time Event Streaming: Kafka vs Redis Streams vs NATS 2026](https://dev.to/young_gao/real-time-event-streaming-kafka-vs-redis-streams-vs-nats-in-2026-34o1)
- [SSE vs WebSockets](https://oneuptime.com/blog/post/2026-01-27-sse-vs-websockets/view)
- [better-auth vs Lucia vs NextAuth 2026](https://www.pkgpulse.com/blog/better-auth-vs-lucia-vs-nextauth-2026)
- [Kiranism/next-shadcn-dashboard-starter](https://github.com/Kiranism/next-shadcn-dashboard-starter)
- [arhamkhnz/next-shadcn-admin-dashboard](https://github.com/arhamkhnz/next-shadcn-admin-dashboard)
- [Vercel Next.js + shadcn/ui Admin Template](https://vercel.com/templates/next.js/next-js-and-shadcn-ui-admin-dashboard)
- [Pycord Slash Commands](https://guide.pycord.dev/interactions/application-commands/slash-commands)
