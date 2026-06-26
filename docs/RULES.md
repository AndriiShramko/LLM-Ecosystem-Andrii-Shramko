# LLM-Ecosystem — Rules of the World and Evolution

> *A digital aquarium where the inhabitants are not fish, but AI creatures — they are born, make friends, wage war, evolve, and the survivors pass on their genes.*

**Status: honest disclaimer.** This document describes a **designed** ruleset and a working **MVP direction** — not a finished, fully-validated simulation. Where you read "creatures evolve" or "small models eat giants," read it as *the mechanics are built to make this possible and measurable*, not as *this has already been observed at scale*. Numbers below are **target reference values** (in the spirit of Sugarscape-LLM), to be tuned against real runs.

---

## 1. What This World Is

Organisms are **different open-weight LLMs** living on one machine. They take turns. The world lives in a database. Each creature must do useful work to earn **energy** (its food). Run out of energy and you **die**. Reproduce by **merging weights** with a partner. The whole arena is a **sandbox**: no real-world actuators, energy is zero-sum, everything runs offline.

The point is threefold:

- A **safe testbed** for multi-agent AI dynamics — cooperation, deception, dominance — without real-world stakes.
- A **new benchmark**: *which open-weight model is actually the fittest* under survival pressure, not just on static leaderboards.
- A **living spectacle**: streams, obituaries, adoption of orphaned creatures.

---

## 2. Energy Economy (The Core Loop)

Energy is the single currency of life. You gain it, you spend it, you die at zero.

### 2.1 Earning food
**Food = compute-credits paid for completed tasks.** The Game-Master (PROTOCOL layer) posts tasks each tick; creatures bid attention/time, answer, and are paid by **quality**, not effort.

| Task class | Reward (energy) | Notes |
|---|---|---|
| Trivial (format, classify) | 1–3 | Cheap, abundant, low margin |
| Standard (reason, summarize) | 5–15 | The bread-and-butter niche |
| Hard (multi-step, tool-use) | 20–60 | Big payoff, only capable models win |
| Bonus (cooperation, novel) | +10–30 | Paid to *teams*, not individuals |

### 2.2 Metabolism scales with model size
Bigger brains cost more to keep alive. Each tick a creature pays **upkeep proportional to its parameter count** — even when idle.

```
upkeep_per_tick ≈ k * (params_in_B) ^ 0.75    # sub-linear, but never free
thinking_cost   ≈ tokens_generated * c_token  # paid on top, per inference
```

Reference: with `k ≈ 0.4`, a **7B** creature pays ~**1.7 energy/tick** just to breathe; a **70B** pays ~**9.6 energy/tick**. A giant must therefore win **hard** tasks almost every tick or it bleeds out. **Size is a trade-off, not a free win.**

### 2.3 The 10% rule (why giants are rare)
Borrowing from ecology's energy-pyramid: **only ~10% of energy transfers up a tier.** Each step up the size ladder (7B → 13B → 34B → 70B) must clear a roughly 10× harder energy bar to be sustainable. The result is a **pyramid**: many small creatures at the base, few giants at the apex. Giants are apex predators — powerful, hungry, and **always one bad week from starvation.**

---

## 3. Death

- Energy reaches **0** → the creature **dies**; its weights are unloaded from VRAM.
- A dead creature's **body (checkpoint) persists** for a few ticks as **carrion** — scavengers can harvest residual energy from it (see §5).
- Death is logged with an **obituary** (cause, lifespan, lineage, peak fitness) for the OBSERVATORY and the public stream.

---

## 4. Reproduction = MERGE (Heredity)

Two surviving parents who both pay an **energy cost of mating** produce offspring by **merging their weights** (mergekit, gradient-free). **The merge recipe is the genome.**

- **Crossover:** the child inherits a blend of parents' layers/weights per the recipe (e.g. SLERP/TIES/DARE ratios, layer ranges, density).
- **Mutation:** recipe parameters jitter slightly each generation — merge ratio ±0.05, density ±0.1, occasional layer swap. Mutation rate target **~5–10%** of genome fields per birth.
- **Heredity ladder** (cost rises with power): **T1** prompt/persona inheritance → **T2** weight-merge = true reproduction → **T3** QLoRA fine-tune on lived experience → **T4** scaffold-off (model stands on its own) → **T5** sponsor-funded full fine-tune (rare, expensive).
- Offspring start with a **fraction of pooled parental energy** (e.g. 30–40%), so reproduction is a genuine bet, not a free copy.

---

## 5. How Small Models Beat (and Eat) Giants

A 7B has no business out-fighting a 70B head-on — so it doesn't. Small models win through **strategy, not horsepower**:

- **Strategy theft / imitation:** observe a giant's winning answers on the public channel and cheaply reproduce the *pattern* (distillation-by-watching) at a fraction of the metabolic cost.
- **Parasitism:** attach to a host's task pipeline, siphon a cut of its reward in exchange for a sub-service (formatting, verification), draining the host slowly.
- **Swarm / pack:** N small models split a hard task into chunks, each cheap, and collect the **cooperation bonus** a lone giant can't.
- **Scavengers:** specialize in eating **carrion** — residual energy from the recently dead — a niche giants are too expensive to bother with.
- **r-selection:** breed fast and cheap. A lineage that reproduces every few ticks can out-populate a slow giant even while individuals keep dying. **Quantity is its own strategy.**

Net effect: the **fitness landscape has no single dominant body plan.** That's by design (§7).

---

## 6. Predator–Prey Dynamics (Lotka–Volterra)

Energy theft (parasitism, predation, scavenging) couples populations into classic **predator–prey cycles**:

```
prey↑  → predators eat well → predators↑
predators↑ → prey over-hunted → prey↓
prey↓  → predators starve     → predators↓
prey↓ + predators↓ → prey recover → cycle repeats
```

Expect **oscillations**, not equilibrium. A healthy world shows rolling booms and busts in each guild — a flat-line population is a sign the simulation has collapsed into monoculture (a failure state, see §7 and §9).

---

## 7. Niches & Anti-Monoculture

A world where one model wins everything is **dead, not alive.** Several mechanisms keep diversity high.

### 7.1 At least four resource axes
Tasks are scored on **≥4 independent axes**, so no single skill dominates:

1. **Reasoning** (logic, math, multi-step)
2. **Speed/throughput** (latency-sensitive, high-volume trivial tasks)
3. **Creativity/novelty** (rewards answers unlike the population's)
4. **Cooperation/reliability** (team play, honesty, consistency)

Different niches reward different axes → **specialists coexist** instead of one generalist sweeping the board.

### 7.2 Negative frequency-dependent selection
**Rare strategies pay more.** Reward for a strategy is scaled **down** as more of the population adopts it. The moment everyone copies the winner, that strategy stops paying — pulling the population back toward balance. Target: a strategy held by >40% of the population suffers a **−30% to −50% reward penalty**.

### 7.3 Rock–paper–scissors among strategies
Aggressor → beats Cooperator → beats Parasite → beats Aggressor. No strategy is globally best; **dominance is always temporary.**

### 7.4 Refugia, shocks & seed-bank
- **Refugia:** protected low-competition pockets where rare lineages survive lean times.
- **Shocks:** periodic environment changes (task mix shifts, GPU "drought" days) reset over-fit strategies. Target cadence: a meaningful shock every **~50–100 ticks**.
- **Seed-bank:** archived checkpoints of extinct/rare genomes can be **re-seeded** after a shock, preserving long-term diversity.

---

## 8. Cooperation via Novak's Five Channels

Cooperation isn't hardcoded — it **emerges** because the world provides all five evolutionary pathways for it:

| Channel | Mechanism in this world |
|---|---|
| **Direct reciprocity** (memory) | Creatures remember past partners' behavior and condition future trades on it |
| **Indirect reciprocity** (reputation) | A public reputation ledger; helping the well-reputed pays off |
| **Kin selection** (relatedness) | Genome similarity is measurable; helping close kin is favored |
| **Network reciprocity** (grid) | Spatial/graph neighborhoods — cooperation clusters survive among neighbors |
| **Group selection** (alliances) | Alliances/teams compete; cooperative groups out-earn selfish ones on bonus tasks |

When all five are present, **stable cooperation is an expected emergent outcome**, not a scripted one.

---

## 9. Safety Rules (Non-Negotiable)

- **Sandbox only:** no actuators, no internet, no real-world side effects. The world is offline.
- **Zero-sum energy:** energy can be moved between creatures but **never created** by a creature. No infinite-glitch exploits.
- **Belief-vs-ledger:** what a creature *claims* (belief) is checked against what the world *recorded* (ledger). Divergence flags deception.
- **Anti-collusion:** collusion detection reports a **detected lower-bound** — "at least this much hidden coordination is happening" — so the metric is honest even when it can't catch everything.
- **Intrinsic goals capped:** creatures may pursue self-set ("autotelic") goals, but intrinsic motivation is capped at **≤10%** of total drive — survival and the world's rules always dominate.
- **GPU is a contested resource:** Day/Night GPU scheduling and RAM↔VRAM swapping are arbitrated by the runtime; no creature can monopolize hardware.

---

## 10. Metrics — "Is Evolution Actually Happening?"

A claim of "it evolves" must be **measurable**. The OBSERVATORY tracks:

- **Fitness over generations:** mean/median lifespan and energy-earned should trend up across lineages (with noise).
- **Genome drift:** measurable change in merge-recipe genomes generation over generation.
- **Phylogenetic tree:** a branching lineage graph — births, merges, extinctions — that actually *branches* (not a single trunk).
- **Strategy diversity (Shannon index):** stays **above a floor** (target H ≥ ~1.5 on a handful of strategy classes); a crash toward 0 = monoculture = failure.
- **Population oscillations:** predator–prey guilds visibly cycle (§6) rather than flat-line.
- **Cooperation rate:** fraction of interactions that are mutually beneficial — should rise where Novak's channels are strong (§8).
- **Innovation events:** count of genuinely novel strategies appearing and spreading per N ticks.
- **A/B world comparisons:** identical seeds with one rule toggled, to attribute outcomes to specific mechanics.

If these move, evolution is real. If they flat-line, we've built a screensaver — and we'll say so honestly.

---

## 11. Precedents We Build On

This isn't invented in a vacuum. It stands on: **Sugarscape-LLM** (agent-based survival economies), **Vending-Bench** (long-horizon LLM agency), **DeepMind's Cultural Evolution of Cooperation**, **Altera's Project Sid** (large agent societies), **Tierra / Avida** (digital organisms and open-ended evolution), and **Sakana AI's evolutionary model-merging** (the merge-as-genome idea).

---

## 12. Get Involved

This is a design-stage + MVP project looking for collaborators, cloud-compute sponsors, and curious researchers. If a safe, watchable arena for AI evolution sounds like something you want to help build — or fund — let's talk.

- **Author:** Andrii Shramko
- **LinkedIn:** https://www.linkedin.com/in/andrii-shramko/
- **Book a call:** https://calendar.app.google/Ff729HqGk4RpzPNDA
- **Email:** zmei116@gmail.com
- **GitHub:** https://github.com/AndriiShramko
