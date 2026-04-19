# Joe AI External-Helper Architecture — Research Brief

_Layer-2 re-research (R1) run 2026-04-18. Summary folded into `../warm-iverson.md` Workstreams D + E._

Targeting NeoForge 1.21.1 + Sinytra Connector, MCP-over-RCON, Mineflayer bots, DGX Spark inference, 2-4 month solo ship.

## 1. Recommended architecture per feature

| Feature | Recommendation | Notes |
|---|---|---|
| Build-preview highlights | **C (Litematica/Forgematica + Syncmatica)** | Generate `.litematic` server-side; Syncmatica pushes to opted-in clients. Non-mod players get a **BlueMap** ghost polygon/POI fallback. |
| Undo overlays | **A + C hybrid** | Chat summary via MCP ("Reverted 42 blocks"), plus a **BlueMap shape marker** (red outline) auto-cleared after N seconds. No in-world ghosting. |
| Vision (bot's own view) | **C — `prismarine-viewer` first-person** | Already supports 1.21.1; stream to a hidden HTTP endpoint, sample frames into the VLM on DGX. |
| Vision (player's view) | **A — defer / opt-in OBS/Discord stream scrape** | No mainstream 2025 mod exposes remote player viewport without custom client code. Use Discord screen-share as voluntary channel. |
| Voice | **Hybrid A+C** — Discord bot (discord.js voice) is primary; a tiny **SVC server-side plugin** bridges into the SVC network via `VoicechatServerApi` entity channels for the bot "speaking near" players. |
| Permissions / approval UX | **A — Discord slash commands + web app** | MCP-over-RCON chat for quick `/joe yes`, but real approval lives in a Discord ephemeral embed with buttons (Auth.js session joins Discord ID to MC UUID). |
| Setup wizard | **A — Next.js onboarding** | First-run wizard at `/setup` over Auth.js; writes Redis config. No in-game UI. |
| RegistryHints | **A — server-side data extraction** | Use `/neoforge tags`/`/data get` via RCON plus a one-time export run to JSON; cache in Redis. No mod needed. |

## 2. Concrete mod + API inventory (consumed, not built)

- **Litematica-Forge / Forgematica** for NeoForge 1.21.1 (ThinkingStudios port) — client-side schematic ghost render.
- **Syncmatica** — server-side schematic distribution so Joe AI can push `.litematic` files without asking players to drag files.
- **Servux** — optional companion to Litematica for accurate placement/NBT sync.
- **BlueMap** + **BlueMapAPI 2.7.x** markers (POI/shape/line) — out-of-game preview, undo outlines, waypoint pins. Non-persistent, re-emit on reload.
- **Xaero's Minimap** + **Server Waypoint** plugin (or XaeroShare) — in-client waypoint pins without custom mod code; chat-sharing accepted server-side.
- **Simple Voice Chat** + `voicechat-api` (`VoicechatServerApi`, `createEntityAudioChannel`, `createAudioPlayer`, `AudioSender`) — locational TTS via a hidden armor-stand "Joe speaker" entity; mic capture via `AudioSender`.
- **prismarine-viewer** (`firstPerson: true`, frame streaming) — bot-perspective vision frames for the VLM.
- **Mineflayer** (1.21.1 supported) — protocol-level actuation; pairs with MCP-over-RCON for commands that need server-authority.

## 3. What must be custom-built if we refuse in-world mods entirely

Minimum viable custom components (all external, Node/TS + Python on DGX):

- **`joe-svc-bridge`** — a single ~200-line NeoForge plugin-style jar that hosts a SVC API plugin. It is not "our Fabric mod" — it ships no UI, no mixin, no client classes. It just exposes an HTTP/gRPC endpoint that accepts PCM and routes to `createEntityAudioChannel`/`AudioSender`. Without this, SVC cannot accept external audio; the API requires in-JVM calls.
- **`joe-schematic-emitter`** — server-side generator that converts agent build plans into `.litematic` files written to Syncmatica's share dir.
- **`joe-bluemap-gateway`** — tiny Java/Kotlin mod or Paper-style hook using BlueMapAPI to re-emit markers from Redis on each BlueMap reload.
- **`joe-vision`** — Node service wrapping `prismarine-viewer` headlessly (puppeteer or node-canvas three.js stream), exposing `/frame` to the VLM.
- **`joe-voice`** — Node service: discord.js v14 `VoiceReceiver` → Opus/PCM → Whisper; Piper PCM → `AudioResource` into Discord voice, fan-out copy to `joe-svc-bridge` when targeting in-world Simple Voice Chat.
- **`joe-web`** — Next.js 16 + Auth.js v5 + Redis Streams for setup wizard, approval UX, registry-hints admin.

The SVC bridge is the only *client-touching* Java code and is feature-flagged off; everything else is Node/Python.

## 4. Voice pipeline design

```
                              ┌──────────────────────────┐
      Player mic (Discord) ─► │ discord.js VoiceReceiver │──► Opus
                              │  (GuildVoiceStates intent)│     │
                              └──────────────────────────┘     ▼
                                                      ┌───────────────────┐
                                                      │ prism-media decode│  PCM s16le 48k
                                                      └───────────────────┘
                                                               │
                                        ┌──────────────────────┴───────┐
                                        ▼                              ▼
                              ┌───────────────────────┐     ┌──────────────────────┐
       Player mic (SVC) ───►  │ SVC AudioSender hook  │     │ faster-whisper server │
                              │ → PCM via joe-svc-br. │     │ (CTranslate2 / CUDA) │
                              └──────────┬────────────┘     └──────────┬───────────┘
                                         │                             ▼ text
                                         └─────────► Redis Stream ► Joe LLM (DGX)
                                                                       │ text
                                                           ┌───────────▼────────────┐
                                                           │ piper1-gpl streaming   │
                                                           │   WebSocket → PCM16    │
                                                           └───────────┬────────────┘
                                                                       │ PCM
                                        ┌──────────────────────────────┼────────────────────────────┐
                                        ▼                              ▼                            ▼
                            ┌─────────────────────┐     ┌──────────────────────────┐   ┌───────────────────────┐
                            │ discord.js AudioRes │     │ SVC createAudioPlayer    │   │ fan-out to transcript │
                            │ playStream→VC       │     │ + EntityAudioChannel on  │   │ text channel (logs)   │
                            └─────────────────────┘     │ armor-stand "Joe"        │   └───────────────────────┘
                                                        └──────────────────────────┘
```

Rationale: discord.js voice is the authoritative mixing layer (players agreed on Discord as primary). SVC is a mirror driven by the same PCM stream so the agent sounds locational to players who prefer in-game. Piper is moving to **piper1-gpl** (archived Oct 2025); plan on that fork. Faster-whisper + SimulStreaming for STT on DGX — single A100-class GPU is massive overkill, letting you run large-v3 with <300ms chunked latency.

## 5. Risks for a 2-4 month solo ship

- **Sinytra Connector beta on 1.21.1** — Litematica/Syncmatica/SVC API are Fabric-native. Connector 2.0 beta compatibility list must be spot-checked for each; one of them failing blocks whole legs.
- **SVC API requires JVM-resident plugin.** If you truly refuse custom Java, you lose in-world voice; Discord-only is workable but worse UX. The 200-line bridge is the smallest retreat.
- **Discord voice receive** needs `GuildVoiceStates` + careful per-user SSRC demux; concurrent speakers + 10 players stresses STT backpressure.
- **prismarine-viewer** is WebGL-based; headless frame capture needs puppeteer/node-canvas and is the least battle-tested piece.
- **Piper archival** — confirm piper1-gpl voice-model compatibility before training the agent around specific voices.
- **Litematica approval UX** — players must accept pasting; Joe can show the ghost but cannot place without player action unless you also run a creative-op bot, which conflicts with permissions design.
- **Servux Velocity proxy bug** (open issue #39) — if you ever add a proxy, schematic sync breaks.
- **BlueMap marker persistence** is plugin-managed; a crash loses markers — must replay from Redis on reload.

## 6. Open questions

1. Is the "no in-world mod of our own" rule absolute, or does the ~200-line SVC bridge plugin count as acceptable since it has no client component and no player-facing UI?
2. Do all 10 players already run Simple Voice Chat, or is Discord voice the true universal channel and SVC an opt-in subset?
3. Is vision required at launch, or can "bot-perspective only" (prismarine-viewer) ship and player-perspective wait for a v2?
4. For build previews, is Litematica client install mandatory for participating players, or must non-Litematica players still get *some* preview (BlueMap fallback needed)?
5. Do we need multi-voice (per-agent persona) TTS, or a single "Joe" voice for Release?
6. Will the Next.js web app ever run outside LAN? If yes, Auth.js v5 + Discord OAuth mapping to MC UUID needs a verification flow (in-game code).
7. Any red line on GPLv3 code pulling into the stack? (Litematica and piper1-gpl are GPL; SVC is LGPL; Mineflayer MIT.)
8. Is a read-only BlueMap "Joe's plan" overlay acceptable as the primary undo UX, accepting that players without the web map open won't see it?

Relevant local project files for grounding: `/home/user/AIminecraftBOT/README.md`, `/home/user/AIminecraftBOT/docs`, `/home/user/AIminecraftBOT/mcp-sidecar`, `/home/user/AIminecraftBOT/run-mcp-sidecar-node.js`.

## Sources

- [Simple Voice Chat API Overview](https://modrepo.de/minecraft/voicechat/api/overview)
- [Simple Voice Chat API Examples (EntityAudioChannel, AudioSender)](https://modrepo.de/minecraft/voicechat/api/examples)
- [voicechat-api-bukkit example plugin](https://github.com/henkelmax/voicechat-api-bukkit)
- [voicechat-broadcast-plugin](https://github.com/henkelmax/voicechat-broadcast-plugin)
- [Simple Voice Chat Discord Bridge (Modrinth)](https://modrinth.com/plugin/simple-voice-chat-discord-bridge)
- [prismarine-viewer (GitHub)](https://github.com/PrismarineJS/prismarine-viewer)
- [Mineflayer](https://github.com/PrismarineJS/mineflayer)
- [Litematica-Forge (ThinkingStudios NeoForge 1.21.1 port)](https://github.com/ThinkingStudios/Litematica-Forge)
- [Forgematica on Modrinth](https://modrinth.com/mod/forgematica)
- [Syncmatica (server-shared litematics)](https://github.com/End-Tech/syncmatica)
- [Litematica schematic pasting wiki](https://github.com/maruohon/litematica/wiki/Schematic-Pasting)
- [BlueMap 3rd-party addons](https://bluemap.bluecolored.de/3rdPartySupport.html)
- [BlueMap Markers docs](https://bluemap.bluecolored.de/wiki/customization/Markers.html)
- [BlueMapAPI Wiki](https://github.com/BlueMap-Minecraft/BlueMapAPI/wiki)
- [Server Waypoint (Xaero sync plugin)](https://modrinth.com/plugin/server_waypoint)
- [XaeroShare](https://modrinth.com/plugin/xaeroshare)
- [Sinytra Connector compatibility matrix](https://connector.sinytra.org/compatibility)
- [discord.js Voice Receiver](https://discord.js.org/docs/packages/voice/main/VoiceReceiver:Class)
- [discord.js Voice Connections guide](https://discordjs.guide/voice/voice-connections)
- [faster-whisper](https://github.com/SYSTRAN/faster-whisper)
- [whisper.cpp](https://github.com/ggml-org/whisper.cpp)
- [whisper_streaming / SimulStreaming](https://github.com/ufal/whisper_streaming)
- [piper-streaming (WebSocket PCM16 server)](https://github.com/jitendraparande/piper-streaming)
- [LiveKit + Piper low-latency voice agent writeup](https://medium.com/@mail2chasif/livekit-piper-tts-building-a-low-latency-local-voice-agent-with-real-time-latency-tracking-92a1008416e4)
- [RealtimeTTS library](https://github.com/KoljaB/RealtimeTTS)
- [Servux+Velocity proxy issue #39](https://github.com/sakura-ryoko/servux/issues/39)
