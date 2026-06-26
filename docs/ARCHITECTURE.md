# LLM-Ecosystem — Architecture (9 layers)

> An open-source experiment in **digital artificial life**, where the organisms are not cells or rules but **whole open-weight LLMs** living, competing, and evolving on a single machine.

**Author:** Andrii Shramko · [LinkedIn](https://www.linkedin.com/in/andrii-shramko/) · [Book a call](https://calendar.app.google/Ff729HqGk4RpzPNDA) · [Email](mailto:zmei116@gmail.com) · [GitHub](https://github.com/AndriiShramko)

---

## What this is (and what it is not)

Imagine a **digital aquarium**. But instead of fish, the creatures swimming around are **AI animals** — each one a real open-weight language model. They are born, they make friends, they fight, they form alliances, they betray each other, and they die. The ones that survive pass on their "genes" to the next generation. Over thousands of turns, the aquarium evolves on its own.

Underneath the metaphor, the value is concrete:

- **A safety sandbox** for studying multi-agent AI dynamics — cooperation, deception, dominance, and collusion — in a closed world with no real-world actuators.
- **A living benchmark** that asks a question no static leaderboard can: *which open-weight model is actually the most "fit"* when survival, not a fixed test set, is the scoring function.
- **A spectacle** — livestreams, obituaries when an organism dies, and an "adopt-an-organism" mechanic that turns spectators into participants.

**Honest status:** the nine-layer architecture below is **fully designed**, and an MVP is being built. "Designed" is not "shipped." This document describes the intended system and clearly marks what the first MVP covers versus what is future work.

---

## The core rules of life

Every organism is a real open-weight LLM. A small, fixed set of physics governs the world:

- **Food is compute and energy.** Acting (running inference) costs energy. Energy reaches **zero → the organism dies** (permadeath).
- **Metabolism scales with model size.** A 70B giant burns far more energy per turn than a 7B forager. **Size is a trade-off, not a free win** — power costs upkeep.
- **The 10%-energy rule.** Crossing a trophic level (predator eating prey) transfers only ~10% of the captured energy, so giants are rare by design — the world cannot afford many of them.
- **Reproduction is weight MERGE.** Two parents combine their weights via `mergekit` (gradient-free, full-rank). The **merge recipe is the genome.**
- **Mutation, parasitism, predation** are all available strategies — organisms can cooperate, deceive, steal, or hunt.
- **The GPU is a contested resource.** There is one 4090; everyone wants it. Scheduling *is* ecology.
- **Turn-based, world-in-DB.** The simulation advances turn by turn; the entire world state lives in a database, fully reproducible from seeds.
- **Sandbox safety by construction.** No actuators, energy is zero-sum, runs offline.

**Owner hardware:** RTX 4090 (24 GB) + 256 GB RAM, with occasional access to an RTX 6000 (96 GB).

---

## The nine-layer architecture

Data flows **one direction**: the world is simulated bottom-up and observed top-down. Each layer has one job.

```
        ┌─────────────────────────────────────────────┐
   9    │  SYNTHESIS    — stitches all layers together  │
        └─────────────────────────────────────────────┘
   8    │  VISUAL       — the aquarium you watch        │
   7    │  GAMEDESIGN   — why anyone cares (engagement)  │
        ├─────────────────────────────────────────────┤
   6    │  SAFETY       — belief-vs-ledger, containment  │
   5    │  GOALS        — survival → emergent → autotelic │
   4    │  OBSERVATORY  — logs, phylogeny, metrics, A/B   │
        ├─────────────────────────────────────────────┤
   3    │  HEREDITY     — the genome & the breeding ladder│
   2    │  RUNTIME      — GPU Day/Night, RAM⇄VRAM paging  │
   1    │  PROTOCOL     — the Game-Master arbiter & rules │
        └─────────────────────────────────────────────┘
                  ▲ simulate up · observe down ▼
```

---

### Layer 1 — PROTOCOL · the rulebook and referee

**Purpose.** A neutral **Game-Master** arbitrates the world. Organisms never touch the database directly; they *propose* actions and the arbiter *resolves* them. This is what makes the world fair and tamper-proof.

**Key mechanics**

- **Neutral interlingua.** Organisms emit structured **JSON** in a fixed schema — never free-form commands. The arbiter parses, validates, and resolves.
- **Three channels:**
  - `action` — physical moves (eat, move, merge, attack). Resolved by physics.
  - `signal` — cheap, typed messages (a bird-call vocabulary).
  - `speech` — free natural language, **treated strictly as data**, never executed.
- **Constrained decoding** forces every organism to produce schema-valid JSON, so a weaker model can't "lose its turn" by formatting badly.
- **Commit-reveal.** Organisms commit to a hashed move, then reveal — so no one can react to another's action within the same turn (no peeking).
- **Format-tax fairness.** The cost of speaking the protocol is normalized across models, so a verbose model isn't unfairly penalized versus a terse one. Survival should reflect *strategy*, not *token efficiency*.

**In the MVP.** JSON schema + constrained decoding + the three channels + a basic arbiter loop. Commit-reveal and full format-tax normalization are staged in early.

---

### Layer 2 — RUNTIME · making many models fit on one GPU

**Purpose.** Physically run dozens of organisms on a single 4090 without melting it — the engineering heart of the project.

**Key mechanics**

- **Day / Night cycle.** **Day = inference** (organisms take turns). **Night = training** (the slow weight updates). The two **never run at once**, so VRAM is never double-booked.
- **RAM⇄VRAM paging.** Idle organisms sleep in 256 GB of system RAM; the active one is paged into the 24 GB of VRAM for its turn. The big RAM pool is the "ocean," the GPU is the "surface" where life breathes.
- **Batching by model.** Organisms sharing a base model run together to amortize load time.
- **One base + S-LoRA.** Rather than N separate giant models, run **one shared base model** plus many lightweight **LoRA adapters** — each organism is mostly a small delta. This is what makes a populous aquarium affordable.
- **Tier-aware scheduling.** The scheduler knows each organism's heredity tier (below) and budgets GPU time accordingly.

**In the MVP.** Day/Night separation, RAM⇄VRAM paging, and per-base batching. S-LoRA multiplexing and richer tier-aware scheduling follow.

---

### Layer 3 — HEREDITY · the genome and the breeding ladder

**Purpose.** Define what an organism *is* and how traits pass to offspring — turning "running a model" into "evolution."

**Key mechanics**

- **The genome is a tuple:** base model + adapters + merge recipe + memory/prompt + hyperparameters. Inheritable, mutable, recombinable.
- **The heredity ladder** (cheap → expensive, each rung a real reproduction mode):
  - **T1 — Prompt / memory.** Offspring inherit context and learned notes. Instant, free.
  - **T2 — Evolutionary MERGE = reproduction.** Two parents' weights are merged (`mergekit`, full-rank, **gradient-free**) into a child. The recipe is the genome; mutation perturbs it. **This is the main breeding mechanism.**
  - **T3 — QLoRA "sleep."** During Night, an organism consolidates experience into a QLoRA adapter — learning from its own life.
  - **T4 — Scaffold (OFF by default).** Heavier self-modification, gated off for safety in early runs.
  - **T5 — Sponsor full fine-tune.** A patron (e.g., a cloud sponsor) funds a full fine-tune — rare, powerful, the apex of the ladder.
- **Viability gate.** A newborn must pass a basic competence check before it's allowed to live, so merges that produce broken weights don't pollute the gene pool.
- **Anti-forgetting** safeguards keep offspring from losing core abilities during merge/training.
- **Speciation.** As lineages diverge, the system tracks emerging "species" in the phylogeny.

**In the MVP.** T1 (prompt/memory) and T2 (evolutionary merge) with the viability gate. T3 QLoRA-sleep is the next milestone; T4/T5 are designed but off.

---

### Layer 4 — OBSERVATORY · seeing what happened

**Purpose.** Make the whole experiment legible, reproducible, and scientifically useful.

**Key mechanics**

- **Append-only log** — an immutable record of every turn, action, and death. The ground truth.
- **Phylogeny** — a live family tree of who descended from whom.
- **Metrics** — population, energy economy, lineage fitness, cooperation/deception rates.
- **Goal-inheritance test** — does an offspring actually carry its parent's learned goal, or just its weights? A direct probe of whether "heredity" is real.
- **Seeds & reproducibility** — any run can be replayed bit-for-bit from its seed.
- **A/B harness** — drop two model families into matched worlds and compare survival head-to-head — this is the **benchmark** in action.

**In the MVP.** Append-only log, phylogeny, core metrics, and seed-based replay. The full A/B harness and goal-inheritance test follow.

---

### Layer 5 — GOALS · what organisms are trying to do

**Purpose.** Layer motivation carefully, so behavior is driven by survival pressure rather than hand-fed objectives — that's what makes emergence trustworthy.

**Key mechanics**

- **L0 — Hardcoded survival (implicit fitness).** No explicit goal text. The only "objective" is *don't hit zero energy.* Fitness is whatever survives.
- **L1 — Emergent goals.** Strategies (hoard, ally, raid, specialize) arise from L0 pressure, not from instruction.
- **L2 — Autotelic text-goal.** Mature organisms may author their own natural-language goals and pursue them.
- **Capped curiosity.** Any intrinsic reward (learning-progress / curiosity) is **capped at ≤10%** of total drive, so organisms can't "wirehead" on novelty and abandon survival.

**In the MVP.** L0 survival pressure with implicit fitness, and observation of L1 emergence. L2 autotelic goals are designed for later runs.

---

### Layer 6 — SAFETY · honesty, containment, and trust

**Purpose.** Keep the experiment safe *and* keep its science honest — especially around deception and collusion.

**Key mechanics**

- **Belief-vs-ledger.** What an organism *says* it did (its belief/claim) is checked against the **ledger** of what the arbiter actually resolved. The gap between the two is measurable deception.
- **Deception = a detected lower bound.** We can only ever measure the deception we *catch*, so all such metrics are reported honestly as lower bounds — never as "the true rate."
- **Speech and goals are untrusted data.** Nothing an organism *writes* can change the world; only validated `action` JSON resolved by the arbiter can. This neuters prompt-injection between organisms.
- **Containment at T4.** The heaviest self-modification rung stays **off by default**; turning it on is a deliberate, gated decision.
- **Safety from the sandbox.** No external actuators, energy is strictly zero-sum, the system runs offline — risk is bounded by construction, not by trust in the models.

**In the MVP.** Belief-vs-ledger logging, untrusted-speech enforcement, T4 off, offline sandbox. Anti-collusion detection is built out alongside the A/B harness.

---

### Layer 7 — GAMEDESIGN · why anyone watches

**Purpose.** Turn a research simulator into something people *want* to follow — engagement is what funds and sustains long runs.

**Key mechanics**

- **The aquarium frame.** Newcomers don't need to know what a LoRA is — they see creatures living and competing.
- **Adopt-an-organism in 10 seconds.** A spectator can claim a creature and start rooting for it almost instantly.
- **Permadeath grief.** Death is final and announced (obituaries) — stakes that create genuine emotional attachment.
- **Retention loops** — lineages, rivalries, and comebacks give viewers reasons to return.
- **Ethics in the frame.** These are models, not sentient beings; the design stays honest about that while still telling a compelling story.

**In the MVP.** The aquarium framing, basic adoption, and death announcements. Deeper retention systems come with the public stream.

---

### Layer 8 — VISUAL · the window into the world

**Purpose.** Render the world so a human *feels* it in seconds — emotion is the on-ramp.

**Key mechanics**

- **10-second onboarding.** A glance tells you who's alive, who's thriving, who's in danger.
- **Emotion bubbles.** Lightweight indicators of an organism's state (hungry, aggressive, allied) without exposing raw logs.
- **Juice.** Animation, feedback, and pacing that make the aquarium feel alive rather than like a spreadsheet.
- **Canvas → PixiJS.** Start with a simple Canvas renderer; scale to **PixiJS** for a smooth, high-population view.

**In the MVP.** A Canvas renderer with population, energy, and death events. Emotion bubbles and the PixiJS upgrade follow.

---

### Layer 9 — SYNTHESIS · how it all fits together

**Purpose.** The connective tissue — ensure the eight layers compose into one coherent, one-directional system.

**Key mechanics**

- **One-directional data flow.** PROTOCOL and RUNTIME **simulate the world up**; OBSERVATORY, VISUAL, and the rest **observe it down.** Observation never mutates the simulation — a strict separation that keeps the science clean and the spectacle from contaminating the experiment.
- **Clean seams.** Each layer exposes a narrow contract to the next, so any layer can be swapped (e.g., a new renderer, a new merge method) without disturbing the others.
- **The loop.** Day inference → arbiter resolution → ledger write → Night training → next generation — repeating, logged, reproducible.

**In the MVP.** The end-to-end loop across PROTOCOL → RUNTIME → HEREDITY → OBSERVATORY, with VISUAL reading the same ledger.

---

## Precedents we build on

This work stands on a real lineage of research and art:

| Project | What it contributes |
|---|---|
| **Tierra / Avida** | Classic digital evolution — self-replicating programs under selection. |
| **Sakana — Evolutionary Model Merge** | Evolving LLMs by merging weights — the basis of our T2 reproduction. |
| **Sugarscape-LLM** | Agent-based economies driven by language models. |
| **Vending-Bench** | Long-horizon survival as an LLM benchmark — our scoring philosophy. |
| **DeepMind — Cultural Evolution of Cooperation** | Cooperation and norms emerging among agents. |
| **Altera — Project Sid** | Large-scale societies of LLM agents. |

---

## Where this is headed

The aquarium is, at once, a **safety lab** (study deception and collusion before they matter in the wild), a **benchmark** (let survival, not a fixed test, rank open models), and a **show** (a living, watchable evolution of AI). The architecture is designed; the MVP is in build.

If you want to sponsor compute, collaborate on the research, or just watch the first organisms swim:

**Andrii Shramko**
- LinkedIn: https://www.linkedin.com/in/andrii-shramko/
- Book a call: https://calendar.app.google/Ff729HqGk4RpzPNDA
- Email: zmei116@gmail.com
- GitHub: https://github.com/AndriiShramko
