# Warm Iverson — Prior-Art Survey for Self-Healing MC Ops (April 2026)

_Full research report. Summary folded into `../warm-iverson.md` Workstream C._

## 1. Prior-Art Matrix

| Remediation category | Existing prior art | What it covers | What it misses |
|---|---|---|---|
| Crash-loop detect + restart | Pterodactyl Wings `crash.go` (timeout-gated restart), AMP auto-restart, MCSManager, MC Server Soft "elevated" restart, `systemd Restart=always`, SpigotMC watchdog scripts | Process exit → bounded restart with cooldown | No progressive backoff, no "rollback to last-known-good world" semantics |
| TPS-drop triage | Spark (`/spark tps`, scheduled profilers, SparkWebAPI plugin), Observable (entity/BE profile), LagGoggles (legacy), Nimbus references are sparse | On-demand + scheduled profile snapshots, heap dumps | No automatic trigger on TPS<threshold → snapshot → correlate-with-change-log pipeline |
| Config drift | None shipping. ATM/SkyFactory runbooks say "redeploy pack" manually | Manual pack re-extract | No diffing against pinned-pack hash, no selective restore |
| Disk guardrails | Pterodactyl backup API + S3 driver (local-spool limitation), `Fz77z/pterodactyl-automated-backups`, PterodactylNodeBackup | Scheduled snapshots, S3 push | No disk-free SLO or alerting built-in; rotation is per-server limit, not global |
| Backup mods in-process | FTB Backups 2, AromaBackup, Simple Backups | Async in-world snapshots | No off-host shipping, no integrity verification |
| Agent containment | LXC + Proxmox HA, systemd-nspawn patterns | Restart on exit | Not MC-aware |
| Mod quarantine | No live-disable framework. Fabric/Forge hotswap is dev-only (instrumentation-based, class-schema-limited per [Fabric hotswap wiki](https://wiki.fabricmc.net/tutorial:hotswapping)). Disabling = rename `.jar` + restart | Cold disable per restart | No online quarantine; no "isolate mod's event handlers" primitive |
| World rollback | FTB Backups `/backup restore`, manual `.tar.zst` swap, pack-admin runbooks | Snapshot restore | No "last-known-*good*" selection (crash-correlated) |
| LLM-as-SRE for MC | Nothing mature. Hopper-Hacks 2026 agent stubs, MoLing-MCP, OpenAI community Hetzner-deploy demo, SoulFire chatbot plugin; general pattern from [Rootly AI-SRE](https://rootly.com/blog/building-trust-with-ai-agents-in-site-reliability-engineering) and [MSFT Agent Governance Toolkit](https://techcommunity.microsoft.com/blog/linuxandopensourceblog/agent-governance-toolkit-architecture-deep-dive-policy-engines-trust-and-sre-for/4510105) | In-game agents, deploy agents | No diagnostic agent that reads Spark + logs + git diffs |
| K8s/SRE patterns | Liveness/readiness/startup probes, rolling restart, Steadybit chaos probe testing | Generic process health | Not TPS-aware |

## 2. Gap Analysis — What We Have to Build

1. **Diagnostic-agent dispatcher.** No one ships an LLM that ingests a Spark snapshot + `crash-reports/*.txt` + recent `git log` of pack config + Pterodactyl event stream and outputs a ranked hypothesis. Closest is Rootly's SRE-agent pattern; we adapt it for MC.
2. **TPS-drop triage automation.** Spark issue [#130](https://github.com/lucko/spark/issues/130) confirms programmatic profiler start/stop is still a feature request; SparkWebAPI exposes metrics but not triggers. We build the loop: poll `/tps` → if MSPT p95 > N for T seconds → `POST /profiler/start` → upload snapshot to object store → notify agent.
3. **Config-drift auto-restore.** Hash the pinned pack's `config/`, `kubejs/`, `defaultconfigs/` at deploy; diff on boot; restore divergent files unless allowlisted. Nothing open-source does this for MC specifically.
4. **Crash-correlated rollback.** Pterodactyl's crash timer gives us N-restart-in-T window; we extend it with a "which world snapshot predates the first bad crash" selector instead of always restoring the latest.
5. **Online mod quarantine.** True hot-disable is infeasible (ClassLoader constraints). Best we can do: mark mod in a denylist, graceful stop, relaunch with `.jar.disabled`. Build that as a first-class primitive.

## 3. Recommended Stack

- **Panel:** stay on **Pterodactyl**; its Wings crash-detection + backup API + egg model is scriptable. AMP is polished but closed-source and harder to wrap.
- **In-server profiler:** **Spark** (mod on server), poll via `/spark tps --json` or the [SparkWebAPI](https://hangar.papermc.io/lines-of-codes/SparkWebAPI) plugin. Keep **Observable** installed for on-demand entity drill-down when agent requests it.
- **Perf mods baseline:** Clumps, Cull Leaves, plus whatever the Create pack already ships (Ferrite Core, ModernFix).
- **Backups:** **FTB Backups 2** inside the JVM for fast world-only snapshots (`/backup`), plus **Pterodactyl backup API → S3** for full-server restic-style archives. Rotation via a custom cron that honors a disk-free SLO (Pterodactyl's per-server limit isn't enough).
- **Watchdog:** Pterodactyl Wings crash-detection is the floor. Add a sidecar service (Rust or Go, in LXC) that implements **progressive backoff** (30s → 2m → 10m) and a "3 crashes in 10 min → rollback world" escalation, since Wings itself won't do world restores (see [panel#2495](https://github.com/pterodactyl/panel/issues/2495), [panel#1641](https://github.com/pterodactyl/panel/issues/1641)).
- **Agent runtime:** LXC + `systemd Restart=on-failure`; template-rebuild via Proxmox API if three restarts fail.
- **Diagnostic agent:** bespoke, running on DGX Spark. Tools: Pterodactyl API, Spark snapshot fetcher, git log of pack repo, log tailer, `propose_quarantine(mod_id)` and `propose_rollback(snapshot_id)` actions with human-approval gate for destructive ones.
- **K8s-ish glue:** TPS-aware **liveness analog** (MSPT probe) and **readiness analog** (chunks loaded + players-can-join) following [k8s probe docs](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/); chaos-test them [a la Steadybit](https://steadybit.com/blog/how-to-validate-your-kubernetes-liveness-probes-with-chaos-engineering/).

## 4. Control-Plane Gateway Integration

Self-healing layer publishes a typed event stream (NATS or Redis Streams) with subjects `mc.crash.*`, `mc.tps.*`, `mc.config_drift.*`, `mc.backup.*`, `mc.agent.*`. The web app subscribes for live dashboards and runbook audit trail; the Discord bot subscribes to `*.paged` and `*.remediated` subjects only. Diagnostic-agent outputs land on `mc.agent.hypothesis` with a signed approval URL routed through the gateway's auth.

## 5. Open Questions

- Acceptable **Recovery Point Objective**: are we fine losing the last 10 minutes of world state on rollback, or do we need sub-minute?
- Do players get a **status page**, or only Discord notifications?
- Budget for **LLM calls per incident** — local DGX only, or fall back to a frontier API if the diagnostic agent stalls?
- Is the pack under **git** already (required for drift-restore), or do we need to introduce it?
- How much **auto-action authority** does the agent have before a human gate — auto-quarantine yes, auto-rollback yes, auto-revert-git-commit no?

## Sources
- [Pterodactyl Wings crash.go](https://github.com/pterodactyl/wings/blob/develop/server/crash.go)
- [Panel crash-detection config issue #2495](https://github.com/pterodactyl/panel/issues/2495)
- [Panel auto-restart frozen servers #1641](https://github.com/pterodactyl/panel/issues/1641)
- [AMP CubeCoders guide 2026](https://space-node.net/blog/amp-cubecoders-minecraft-panel-guide-2026)
- [MCSManager](https://www.mcsmanager.com/)
- [MC Server Soft docs](https://docs.mcserversoft.com/basic/server-settings/accessing-server-settings)
- [Spark profiler](https://spark.lucko.me/)
- [Spark programmatic API request #130](https://github.com/lucko/spark/issues/130)
- [SparkWebAPI plugin](https://hangar.papermc.io/lines-of-codes/SparkWebAPI)
- [Spark TPS/MSPT docs](https://spark.lucko.me/docs/guides/TPS-and-MSPT)
- [Observable mod](https://modrinth.com/mod/observable)
- [Akliz Observable guide](https://help.akliz.net/docs/observable)
- [Clumps](https://modrinth.com/mod/clumps)
- [FTB Backups 2](https://www.curseforge.com/minecraft/mc-mods/ftb-backups-2)
- [Simple Backups](https://www.curseforge.com/minecraft/mc-mods/simple-backups)
- [Pterodactyl S3 backup guide](https://pterodox.com/guides/s3-backups.html)
- [Fz77z Pterodactyl automated backups](https://github.com/Fz77z/pterodactyl-automated-backups)
- [Pterodactyl Panel backup system](https://deepwiki.com/pterodactyl/panel/4.3-backup-system)
- [Systemd unit for Minecraft gist](https://gist.github.com/dotStart/ea0455714a0942474635)
- [Watchdog explanation (GGServers)](https://help.ggservers.com/en-us/article/what-is-watchdog-and-why-does-it-crash-my-server-n8aex/)
- [Fabric hotswapping wiki](https://wiki.fabricmc.net/tutorial:hotswapping)
- [ATM9 FAQ](https://allthemods.github.io/alltheguides/atm9/faq/)
- [k8s probes docs](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)
- [Steadybit chaos probe validation](https://steadybit.com/blog/how-to-validate-your-kubernetes-liveness-probes-with-chaos-engineering/)
- [Colin Breck on probes footguns](https://blog.colinbreck.com/kubernetes-liveness-and-readiness-probes-how-to-avoid-shooting-yourself-in-the-foot/)
- [Rootly AI-SRE trust](https://rootly.com/blog/building-trust-with-ai-agents-in-site-reliability-engineering)
- [MSFT Agent Governance Toolkit](https://techcommunity.microsoft.com/blog/linuxandopensourceblog/agent-governance-toolkit-architecture-deep-dive-policy-engines-trust-and-sre-for/4510105)
- [hopper-hacks minecraft-agents](https://github.com/eduardoloz/minecraft-agents)
- [MoLing-Minecraft MCP](https://github.com/gojue/moling-minecraft)
- [SoulFire AI chatbot integration](https://soulfiremc.com/blog/ai-chatbot-integration-testing)
