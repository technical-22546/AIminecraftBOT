# Create: Aeronautics Dependency Verification (2026-04-18)

_Layer-2 verification (R-AeroDeps). Resolves conflicting claims between R3 and R-ATM10 reports. Summary folded into `../warm-iverson.md` Workstreams A + G._

## Question

Is **Valkyrien Skies 2 (VS2)** a hard dependency of **Create: Aeronautics 1.0.2** on NeoForge 1.21.1?

R3 (NF+Sinytra+Mineflayer compat sweep) listed Create: Aeronautics 1.0.2 as safe **without flagging VS2**. R-ATM10 (overlap analysis) claimed VS2 is a hard dep. The two contradict.

## Verdict

**NO. VS2 is not a dependency.** R3 was correct. R-ATM10 conflated Create: Aeronautics with Create: Interactive (a different mod that does depend on VS2 — and is dropped from our pack).

## Create: Aeronautics 1.0.2 (NF 1.21.1) — full dependency list

- **Create** (required)
- **Sable** (required) — the mod's own physics library, NOT VS2
- No optional dependencies listed

Team: Creators-of-Aeronautics org (Eriksonn, RyanHCode, Starlotte, KyanBirb, Kryppers, keyberries, BeeIsYou, Cyvack). Mod description: *"Create Aeronautics expands on the systems of Create to enable players to build all types of vehicles and physics contraptions using Sable."*

## Differentiator: Aeronautics vs Interactive

| Mod | Physics engine | VS2 required? | Status in pack |
|---|---|---|---|
| **Create: Aeronautics** | Sable (own engine) | **NO** | **SHIP** (locked) |
| **Create: Interactive** | VS2 (interactive contraptions on VS ships) | Yes | DROPPED earlier (R3 + Sweep 1 confirmed: 1.20.1 only; Mineflayer-breaking via VS2) |

## Action items

- **Add Sable to required dependency list** for Create Remastered (not VS2).
- **Do not ship VS2** in Create Remastered — no need; would only add Mineflayer-hostile entity-tracking edge cases.
- **R-ATM10 finding superseded:** strike the VS2-hard-dep line; replace with Sable.

## Sources

- [Create: Aeronautics 1.0.2 NF 1.21.1 (Modrinth version page)](https://modrinth.com/mod/create-aeronautics/version/HY8u0JqC)
- [Create: Aeronautics main page (Modrinth)](https://modrinth.com/mod/create-aeronautics)
- [Create: Interactive (VS2 wiki)](https://www.valkyrienskies.org/interactive)
- [Create: Interactive wiki entry](https://valkyrienskies.miraheze.org/wiki/Interactive)
