# Vaultrunner — Design Doc

Oct 10, 2026 · @Csaba

## Concept

A GPS-anchored augmented-reality app: a persistent digital layer over real-world places, pre-populated with **Vaults** and **Forts** — hackable locations anchored to real GPS coordinates. These locations are created by the system and nothing about one is ever hidden from a player who can reach it — its Difficulty and current availability are the same information for anyone who can see it. What's limited is reach itself: a **Rig** (the player's phone, acting as a hacking device) has a sensor that only reveals, and can only act on, what's within its Range — about 100 miles even at the weakest setting, worldwide at the top, so most players will only ever see a subset of the world unless their sensor is maxed out. Physical proximity still matters even for a maxed-out Rig — showing up in person always earns a bonus — and there's real risk either way: every Vault and Fort has a Difficulty, and every failed attempt costs you something.

The loop is discover, approach, hack, earn, upgrade, repeat.

**Terminology:** this design uses original, made-up terms throughout (INARN, Rig, Fort, Vault, Entity, and so on — see Terminology below), so no existing franchise's trademarked names, factions, or iconography need to appear in a shipped product. Art and copy should follow the same rule. **Working name** for the app itself: **Vaultrunner**.

## Terminology

| Term | What it means here |
| --- | --- |
| **INARN** | International Augmented Reality Network — the overall digital overlay, the whole network of Vaults and Forts layered on the real world. |
| **Rig** | The player's hacking device — their phone, in effect. Has four stats: Range, Attack, Defense, and Health (see Rig stats below). |
| **Entity** | A player's avatar/identity as it appears to others in INARN — leaderboards, tipped-off targets, that kind of thing. |
| **Vault** | A common, lower-difficulty hackable location, seeded by the system. Smaller, faster-respawning rewards. |
| **Fort** | A rarer, tougher hackable location, seeded by the system. Bigger rewards, longer respawn. |
| **Difficulty** | A 1–10 rating on every Vault/Fort, fixed when it's seeded (or rerolled on respawn). What your Attack is checked against. |
| **Range** | A Rig's combined sensor and hacking reach — how far away a Vault/Fort can be and still show up on your map *and* be attempted. 1 ≈ about 100 miles (the formula's anchor); 20 ≈ worldwide (sees, and can act on, everything). Most players only ever see a subset of the world — the part within their current Range. |
| **Close Range** | A fixed, universal proximity band — more than 1 mile but at most 5 miles from a target — the same for everyone, regardless of Rig stats. Hacking from here adds a flat +10 percentage points to success chance, but only once your Attack has already cleared that target's hard wall; it can't open the wall itself. |
| **Hack Attempt** | A single try at breaching a Vault or Fort. Resolved by comparing your Rig's Attack to the target's Difficulty, with a proximity bonus if you're within Close Range or Dropping by. |
| **Lockout** | A cooldown placed on a specific Vault/Fort, for the player who just failed against it, after a failed Hack Attempt. Private to that player. |
| **Reboot** | The daily, server-timed reset that restores a Rig's Health to full. |
| **Gadget** | A device upgrade — a reward that raises one of a Rig's stats. |
| **Dropping by** | Attempting a Hack Attempt from at most 1 mile from the target — the tightest band, and the biggest proximity bonus: a flat +20 percentage points. |
| **Projecting** | Attempting a Hack Attempt from beyond Close Range (more than 5 miles out) but still within your Rig's Range — no proximity bonus, but still possible. |

These are working names, not a final glossary yet. What matters is keeping the ideas: a persistent overlay, a world full of hackable targets where nothing about a target is ever hidden from whoever can reach it, and a reach that's earned through gear (Range) but always rewards actually showing up — the closer, the better.

## Design principles

- **Nothing about a target is hidden — only distance gates it.** A Vault or Fort's Difficulty and current cooldown/availability are the same information for every player who can currently see it; there's no private knowledge, no "you found it first so only you know." What's earned through gear is Range itself — how far a Rig can sense *and* act — not secret information.
- **Difficulty creates real risk.** Every Vault and Fort has a fixed Difficulty (1–10) that your Attack is checked against. A strong Rig still faces long odds against a high-difficulty Fort; a weak one can still profitably farm low-difficulty Vaults.
- **No single stat should trivialize the others.** Attack decides the odds of one attempt; Range decides which targets exist for you at all; Health decides how many swings you get per day; Defense decides how much a bad swing costs. A player who only invests in Attack still can't reach distant Forts, still runs out of Health fast against tough targets, and still eats the full cost of every loss.
- **Showing up still matters, even at max Range.** Range gates whether a Hack Attempt is possible from where you're standing at all (1 ≈ about 100 miles, 20 ≈ worldwide), but within that, distance itself tiers the outcome: beyond 5 miles there's no bonus, inside 5 miles (Close Range) is +10%, inside 1 mile (Dropping by) is +20% — regardless of Range. A maxed-Range player hacking from across the world is playing it the hard way, by choice.
- **Even winning costs something.** A successful hack still costs a little Health, deliberately immune to Defense — otherwise a high-Attack Rig could clear every low-Difficulty Vault in reach all day, locking everyone else out of them nonstop for free. Health is the one resource that caps total hacks per day regardless of how strong a Rig gets.
- **Failure costs something, but the cost is capped and resets.** A failed hack costs more Health than a success and puts that target on Lockout for you; Defense softens both, but never to zero. Health always refills at the next Reboot — a bad run costs you the rest of a day, never your account's progress.
- **Rewards are the loop's spine.** Money and Gadgets are what make repeated hacking worth the risk, and what let a Rig grow strong enough to take on harder targets. Harder targets should pay out more.
- **The world is pre-built, not player-built.** Vaults and Forts are seeded by the system, not founded by players.

The next sections work through these in play order: the loop a player repeats, the Rig's stats, how Vaults differ from Forts, how a Hack Attempt resolves, and what it pays out.

## Core hacking loop

1. **Sense** — within your Rig's current Range, nearby Vaults and Forts appear on the map, each showing its Difficulty and current cooldown/availability — nothing about a target's status is ever hidden from anyone else who can also reach it, but even the lowest Range only ever reveals (and can act on) what's within about 100 miles; only a maxed-out sensor sees the whole world. Range is checked against your *current* location, so traveling brings more of the world into range even without upgrading the stat.
2. **Decide how to approach** — hack from wherever you're standing (Projecting, no bonus, as long as you're within Range), close to within 5 miles for a +10% bonus (Close Range), or close all the way to within 1 mile for a +20% bonus (Dropping by).
3. **Decide whether to swing** — the app shows your exact success chance for this target before you commit, including what it'd be at Close Range or Dropping by if you moved closer. If it reads 0% at every distance, your Attack hasn't cleared this Difficulty's wall yet and the Hack action is disabled outright — there's nothing to gamble on. Otherwise, weigh the number against your remaining Health for the day.
4. **Hack** — attempt the hack; Attack vs. Difficulty (and proximity) resolves success or failure (see Discovery, hacking, and failure formulas below).
5. **Outcome:**
   - **Success** — claim the reward (money and/or a Gadget); lose a small amount of Health anyway (unaffected by Defense — see formulas below, this is what stops a strong Rig from clearing targets all day for free); the target goes on a shared respawn cooldown, visible to anyone whose Range can see it, open to any player again once it ends (likely with its Difficulty and reward rerolled).
   - **Failure** — lose a larger amount of Health (reduced by Defense) and the target goes on Lockout for you specifically (shortened by Defense); other players are unaffected and can still try it.
6. **Repeat or rest** — keep hunting until Health hits zero for the day; it refills at the next Reboot.

## Rig (hacking device) stats

**Scale:** Attack, Defense, and Range run **1–20**, not 1–10 — Difficulty stays at the already-established 1–10 (it's content metadata, not a player stat). The finer scale matters for the "slowly upgrade" goal: with only 10 steps, every single Gadget would be a huge jump; with 20, progress can be gradual and each formula below can move in small, legible increments. Health is a separate, bigger pool — recommend a base around 30–50, growable toward 100 via Gadgets — since the per-hit damage numbers below need room to feel graded rather than all-or-nothing.

| Stat | Scale | What it does |
| --- | --- | --- |
| **Range** | 1–20 | A Rig's combined sensor and hacking reach, measured from your current location — how far away a Vault/Fort can be and still show up on your map *and* be attempted. 1 ≈ about 100 miles; 20 ≈ worldwide. |
| **Attack** | 1–20 | Hacking strength. Must clear a Difficulty-specific threshold just to have any chance at all (see formulas below) — below it, success chance is a hard 0%. |
| **Defense** | 1–20 | Mitigates the downside of a failed hack — reduces Health lost and shortens the Lockout. Never reduces either to zero. |
| **Health** | ~30–100 | A daily pool of damage a Rig can absorb. Every attempt costs *some* Health, win or lose (see formulas below). Hits zero → no more Hack Attempts until the next Reboot. |

All four are raised by equipping Gadgets earned from successful hacks — the upgrade loop that makes early, easy Vaults worth running: they fund the gear that lets you reach for Forts. None of the four alone is enough: Attack without Range only ever reaches what's nearby; Range without Attack can reach a Fort but still loses to it; either without Health or Defense runs out of attempts fast against anything hard. The sensor formula (placeholder growth rate, tune freely — `MaxDistance = 100 miles × 1.29^(Range−1)`, capped at "worldwide") is anchored so even the lowest possible Range already covers a wide area:

| Range | Max hacking distance |
| --- | --- |
| 1 | ~100 miles |
| 5 | ~280 miles |
| 10 | ~1,000 miles |
| 15 | ~3,500 miles |
| 20 | Worldwide, no limit |

For reference, a starter Rig might begin around Attack 4, Defense 4, Range 1, Health 40. Even at the lowest possible Range, the sensor already reaches ~100 miles — so what keeps early play local isn't the sensor, it's the Attack wall (only Vaults in the Difficulty 1–3 range are a safe bet at first) and the Dropping-by/Close Range bonuses that reward actually traveling. That's the slow climb this whole system is meant to support.

## Vaults vs. Forts

| | Vault | Fort |
| --- | --- | --- |
| Frequency | Common | Rare |
| Difficulty range | Typically low–mid | Typically mid–high |
| Reward | Smaller, steady | Larger, occasional Gadget drops weighted higher |
| Respawn after a success | Fast | Slow |
| Role in the loop | Grinding target — funds early Gadgets | Aspirational target — needs a built-up Rig |

A new player's early game is mostly Vaults, close to home, within a starter Rig's modest Range and Attack; Forts are the reason to invest in upgrades and travel further.

## Discovery, hacking, and failure formulas

All numbers below are placeholders for playtesting, not a final spec. Success is governed by a **hard wall**, not a steep curve: below a Difficulty-specific Attack threshold, the chance is exactly 0%, full stop — no 1%, no 0.1%. Only once Attack crosses that wall does a real, displayed, climbable percentage exist. Failure cost and Lockout still scale continuously on top of that, so *how bad* a loss is keeps being a gradient even though *whether you can try at all* is not.

### Discovery

A Vault/Fort shows up on your map, and can be attempted, only if it's within your Rig's current reach:

```
MaxDistance(Range) = min( 100 miles × 1.29^(Range−1), EARTH_MAX )   // EARTH_MAX ≈ 12,450 miles
Discovered = (real-world distance to target) ≤ MaxDistance(Range)
```

No dice roll — it's a hard cutoff, and it's deliberately generous even at the bottom: Range 1 already covers ~100 miles, so almost every player can at least *see* most of their region from day one. The growth rate (1.29× per Range point) is the tunable part; the anchor — Range 1 reaches 100 miles, Range 20 reaches worldwide — is fixed.

### Hacking (success chance) — the hard wall

Distance also tiers the Hack Attempt itself, independent of Range or Difficulty — three fixed, ungrowable bands, the same for every player:

```
ProximityBonus(distance) =
    20%   if distance ≤ 1 mile                    (Dropping by)
    10%   if 1 mile < distance ≤ 5 miles            (Close Range)
     0%   if distance > 5 miles, up to MaxDistance   (Projecting)

AttackMin(Difficulty) = round(1.6 × Difficulty)        // Attack scale 1–20, Difficulty 1–10

BaseChance(Attack, Difficulty) =
    0%                                                       if Attack < AttackMin(Difficulty)
    clamp(1 + 7 × (Attack − AttackMin(Difficulty)), 1, 95)%   if Attack ≥ AttackMin(Difficulty)

SuccessChance =
    BaseChance                                           if BaseChance = 0%   (the wall — no proximity bonus can open it)
    clamp(BaseChance + ProximityBonus(distance), 1, 95)%  if BaseChance ≥ 1%
```

A max-Difficulty (10) Vault/Fort needs `AttackMin = 16` — matching the brief exactly — and every lower Difficulty has its own wall a few points lower:

| Difficulty | AttackMin (first 1% chance) |
| --- | --- |
| 1 | 2 |
| 2 | 3 |
| 3 | 5 |
| 4 | 6 |
| 5 | 8 |
| 6 | 10 |
| 7 | 11 |
| 8 | 13 |
| 9 | 14 |
| 10 | 16 |

Both proximity tiers only ever apply *on top of* an already-nonzero `BaseChance` — they're gated by the wall, not a way around it, exactly as specified. The app must show `SuccessChance` before the player commits (and what it would be at each closer distance band); if it reads 0% everywhere, the Hack action is disabled outright rather than letting someone burn Health on a guaranteed, pointless loss.

### Failure costs

Pass/fail now comes from the wall above, but losing against Difficulty 16 at Attack 16 (barely over the wall) should hurt more than losing at Attack 20 (comfortably over it) — so cost still needs its own continuous measure, independent of proximity (distance only ever affects the chance, never the cost of losing):

```
OutmatchGap = (Difficulty × 2) − Attack

HealthLost  = Difficulty + max(0, 0.5 × (OutmatchGap − Defense))
LockoutMin  = (Difficulty × 10) × (1 + max(0, OutmatchGap) × 0.05) × max(0.4, 1 − Defense × 0.03)
```

`HealthLost` always costs at least the target's Difficulty — the floor that makes "scaling damage depending on Difficulty" real, never fully defended away. `LockoutMin` is capped at a 60% reduction from Defense (`Defense = 20` → factor `0.4`), never full immunity.

### Success cost — the anti-griefing piece

```
HealthLostOnSuccess = max(1, round(Difficulty × 0.3))   // 1–3 Health, ignores Defense entirely
```

Every Hack Attempt costs Health, win or lose. This one is small, scales gently with Difficulty, and no stat reduces it — its job is a hard daily ceiling on how many targets *any* Rig can clear, not a risk to defend against. Without it, a maxed-Attack player could clear every low-Difficulty Vault in range all day, repeatedly locking others out for free, without ever touching their Health pool.

### Why this still lets players climb

Attack points spent while still under a wall don't show up as improving odds — they show up as nothing, until the exact point they cross it, at which point a real number appears for the first time. Every point spent above the wall after that adds 7 percentage points of chance while simultaneously shrinking `OutmatchGap`, which cuts the cost of losing too. A Fort that's flatly impossible today becomes possible at a specific, predictable Attack value, and gets cheaper to attempt with each point after that — a visible finish line, not a vague improvement.

**Worked examples**, Difficulty 10 (`AttackMin = 16`):

| Attack | Projecting (>5 mi) | Close Range (1–5 mi) | Dropping by (≤1 mi) |
| --- | --- | --- | --- |
| 14 | 0% — wall, can't attempt | 0% — wall, still can't attempt | 0% — wall, still can't attempt |
| 16 | 1% | 11% | 21% |
| 18 | 15% | 25% | 35% |
| 20 (max) | 29% | 39% | 49% |

Failure cost at Attack 16, Defense 6, Difficulty 10 (`OutmatchGap = 20 − 16 = 4`): `HealthLost = 10 + max(0, 0.5×(4−6)) = 10`; `LockoutMin = 100 × (1 + 4×0.05) × max(0.4, 1−0.18) = 100 × 1.2 × 0.82 ≈ 98 minutes`. A win at the same stats costs `HealthLostOnSuccess = max(1, round(3)) = 3`.

## Rewards

Two reward types are set for now; more can be layered on later:

- **Money** — an in-game currency earned from every successful hack, scaling with the target's Difficulty.
- **Gadgets** — device upgrades that raise Range, Attack, Defense, or Health; drop odds and tier should scale with Difficulty, so Forts are the main source of strong gear.

Reward balance (how much money, how fast Gadgets should accumulate, whether Gadgets have slots/limits) is intentionally undecided — flagged as an open risk below rather than guessed at here.

## Social & sharing

The social layer centers on competition and sharing discoveries:

- **Leaderboards** — total money earned, hardest Fort cracked, longest streak without a Reboot-to-Reboot failure.
- **Tipping off friends** — since a player's own Range only reveals a fraction of the world, an Entity can flag a specific Vault/Fort to friends whose sensor wouldn't otherwise reach it — the main way word spreads about something worth a trip, short of maxing out your own Range.
- **Circles (optional)** — loose interest/region-based groups for comparing leaderboards or sharing tip-offs — not factions with any mechanical edge over each other, consistent with keeping this PvE rather than PvP.

## Technical architecture

&#91;embedded content: system components · client, gateway, services, data\]

The one non-negotiable, unchanged from before: location is **verified server-side**, not trusted from the client. It now gates more than before: the server must compute the real-world distance between a player's current, verified location and a target before it's even shown as sensed (the "what's in Range" query), before allowing a Hack Attempt, and before awarding either proximity tier's bonus (Close Range or Dropping by) — GPS-spoofing would otherwise let someone fake a sensor sweep or a Hack Attempt from anywhere. The Hack Attempt resolution itself — the `AttackMin` wall check, the success-chance calculation, the proximity bonus, Health deduction, and Lockout — must happen server-side too, since a client that computed its own outcome could just always report success. This also covers the pre-attempt success-chance preview players need to see: it has to be a real server computation off the player's actual stats and verified location, not a client-side estimate a modified client could lie about to show a nonzero chance under a wall it hasn't actually crossed.

Other pieces, mostly carried over: a geospatial service (PostGIS-backed) for Vault/Fort placement, the Range-based "what's nearby" sensor query, and the tighter 1-mile/5-mile proximity-tier checks for Hack Attempts; an Entity/Rig/account service tracking stats, Gadgets, Health, and Lockouts; a scheduled job for seeding/respawning Vaults and Forts and for the daily Reboot; push notifications for "a Fort just respawned within your Range" alerts, plus a separate path for a friend's tip-off, since that can point at something currently beyond the recipient's own Range. The AR map view can stay a fairly standard map SDK with camera overlay — full ARKit/ARCore marker tracking is a nice-to-have, not required for V1.

## MVP scope, roadmap, and open risks

**Phase 1 (MVP)** — map with hand-seeded Vaults only (no Forts yet), fixed starter Rig stats (no Gadgets yet), physical-presence-only Hack Attempts, money as the only reward. Proves the sense → travel → hack → earn loop feels good before difficulty curves or upgrades complicate it.

**Phase 2** — Forts, Difficulty-scaled rewards, Gadgets and the four-stat Rig upgrade loop, Lockout and Health tuning from real playtest data.

**Phase 3** — social layer: leaderboards, tipping off friends, Circles.

**Phase 4** — richer target types, live/limited-time Forts, seasonal economy resets.

**Open risks to resolve before building, not after:**

- **IP/trademark** — no logos, faction names, or copy that lifts recognizable phrasing from any existing cyberpunk/hacking franchise; stick to the original terminology defined in this doc.
- **GPS spoofing, raised stakes** — now gates something with real in-game cost (Health, Lockouts, rewards), not just a cosmetic badge, so the server-side distance and location checks (see Architecture) are load-bearing from day one, not a nice-to-have.
- **Economy balance** — money rates and Gadget drop odds are still unset, and the Discovery/hacking/failure formulas are only a first-draft shape; shipping any of it unbalanced risks either a trivial game (everything hackable, no tension) or a punishing one (Health drains faster than it can be earned back). Needs a playtest pass before wide release.
- **Real-world safety** — a map that concentrates players at specific physical coordinates needs the same guardrails Niantic built: no placing targets on private property without consent, and sensible siting rules (don't seed a Fort somewhere unsafe or inaccessible to reach).
- **Seeding fairness** — targets are system-placed, so seeding logic needs review to make sure targets don't cluster unfairly or appear in places that put players at risk chasing them (traffic, restricted areas).
- **Pay-to-win pressure** — if Gadgets can ever be bought rather than only earned, the Attack/Defense/Health stats that gate progress become a monetization lever; worth deciding the stance on this explicitly before Phase 2, not after players notice.
- **Remote play could swallow the "go outside" incentive** — this isn't only an endgame, max-Range concern: even the lowest possible Range already senses ~100 miles out, and Range 20 reaches the whole planet, so a player could in principle never leave home. What's meant to keep play local in practice is the Attack wall (no point sensing a target you can't clear yet) and the Dropping-by/Close Range bonuses — but whether those pulls are strong *enough* (versus just a nice-to-have) is unproven until playtested.
