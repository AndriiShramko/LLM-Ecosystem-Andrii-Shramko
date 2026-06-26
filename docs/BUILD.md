# Building & Running LLM-Ecosystem

> Author: Andrii Shramko — a digital aquarium where the inhabitants are not fish, but open-weight AI organisms that are born, befriend, fight, and evolve.

**Honest status:** This document is the **MVP implementation plan**, not a description of finished software. The 9-layer architecture is *designed*; the Phase-0 MVP described below is what we build first to prove the core loop. Where something is a plan, we say so. Nothing here is "press play and watch" yet — it's the blueprint a contributor or sponsor can pick up and start from.

---

## What you're building

A turn-based artificial-life simulation where each **organism is a different open-weight LLM** running on one machine. Compute and energy are food. Organisms die at energy 0, **reproduce by merging their weights** (the recipe is the genome), mutate, and compete. Metabolism scales with model size, so being big is a trade-off, not a free win.

---

## Tech stack

| Layer | Choice | Why |
|---|---|---|
| Language | **Python 3.11+** | Ecosystem for ML tooling, mergekit, SQLite |
| Inference | **Ollama** (MVP) or **vLLM** (scale) | Ollama = trivial 4-bit local serving; vLLM = throughput + multi-LoRA later |
| Weight merging | **mergekit** | Gradient-free reproduction; the merge recipe *is* the genome |
| World state | **SQLite** | Single-file, transactional, zero-ops; the whole world lives in one DB |
| Dashboard | **Canvas / lightweight web** | Aquarium view + live Event Feed; no heavy frontend framework |

Everything in the MVP runs **offline and open-weight** — a deliberate safety property, not just a budget one.

---

## Hardware tiers

You can start the MVP today on a single high-end consumer GPU. Bigger tiers unlock bigger organisms and real training, but are **not required** to see the core loop alive.

| Tier | Hardware | Cost | Unlocks |
|---|---|---|---|
| **Tier-Local** | 1× RTX 4090 24GB + 256GB RAM | ~$0 (owned) | Full Phase-0 MVP: 5–8 small models, merge-reproduction, survival loop |
| **Tier-Friend** | + RTX 6000 96GB (occasional) | borrowed/occasional | Rare "giant" organisms (≤70B 4-bit), heavier merges, QLoRA experiments |
| **Tier-Sponsor** | 8×80GB cluster | sponsored | Full fine-tuning of ≤70B organisms (the T5 "sponsor" heredity tier) |

The 256GB of system RAM is the secret weapon: models swap between RAM and VRAM, and merges happen **in RAM for free** instead of needing everything resident in VRAM at once.

---

## Phase-0 MVP — step by step

This is the smallest thing that proves the idea is alive. Target: **Tier-Local (one 4090)**.

1. **Pick 5–8 diverse open-weight models.** Different families/sizes (e.g. a 1–3B, a few 7–8B), all pulled through **Ollama in 4-bit (Q4)**. Diversity of "species" is the point.
2. **Define the world grid + survival loop.** A simple grid or shared pool. Each turn, organisms act in a **Donor-Game**-style interaction (cooperate / defect / share energy).
3. **Wire up metabolism = size.** Energy cost per turn is **proportional to model size**. Big models think better but starve faster. Enforce the "10% energy" rule so giants stay rare.
4. **Implement merge-reproduction.** When two organisms reproduce, combine their weights with **mergekit**; the merge recipe is stored as the child's **genome** (heritable, mutatable).
5. **Log everything to SQLite.** Every turn, action, energy delta, birth, death, and merge recipe. The DB is the single source of truth and the audit trail.
6. **Build a simple Canvas dashboard + Event Feed.** Show the aquarium (who's alive, energy bars) and a scrolling feed of births, deaths, alliances, and betrayals.
7. **Run, watch, iterate.** Confirm the loop is *actually* alive end-to-end before adding anything fancy.

> Heredity scope for MVP: **T1 (prompt) + T2 (merge = reproduction)** only. QLoRA (T3) is Phase-2, opt-in, behind a flag. Self-rewriting scaffold (T4) and sponsor full-fine-tune (T5) stay **OFF** by default and are operator-gated decisions.

---

## What NOT to do on a 4090

Honest constraints — ignoring these will stall the MVP:

- **Don't full-fine-tune** any organism on the 4090. Full-FT belongs to Tier-Sponsor (8×80GB).
- **Don't train every organism every turn.** Merge gives heritable adaptation almost for free; per-turn training melts your turn-by-turn loop into hours-per-tick.
- **Don't hold all models in VRAM at once.** Stream models RAM↔VRAM; keep only the active organism(s) resident. 24GB is for *one* organism's turn, not the whole population.

---

## Small-model VRAM reference (Q4 ballpark)

Rough 4-bit footprints to plan how many "species" fit. Treat as order-of-magnitude, not exact.

| Model size | ~VRAM (Q4) | Notes |
|---|---|---|
| 1–2B | ~1–2 GB | Cheap, fast "minnows" — many can coexist |
| 3B | ~2.5–3 GB | Good low-metabolism organisms |
| 7–8B | ~5–6 GB | The MVP sweet spot |
| 13B | ~9–10 GB | Mid-tier; metabolically expensive |
| 30–34B | ~20–22 GB | Near the 4090 ceiling — rare giants only |
| 70B | ~40 GB+ | Needs Tier-Friend (96GB) or 4-bit + heavy swap |

On a 4090, the 7–8B band is where the interesting dynamics live. Anything ≥30B is a deliberate, rare "apex" organism.

---

## Scaling up: vLLM + multi-LoRA

When the MVP proves out and you want many adapted organisms cheaply:

- Move inference from Ollama to **vLLM** for higher throughput and batching.
- Use **multi-LoRA**: serve one base model with many lightweight **LoRA adapters**, each adapter = one organism's learned variation. This lets dozens of "evolved" organisms share a single base in VRAM instead of loading a full model per organism.
- This is the bridge between Tier-Local experiments and Tier-Friend / Tier-Sponsor training runs.

---

## Suggested repository structure

```
LLM-Ecosystem-Andrii-Shramko/
├── README.md
├── docs/
│   ├── BUILD.md            # this file
│   ├── ARCHITECTURE.md     # the 9-layer design
│   └── SAFETY.md           # sandbox model, zero-sum energy, anti-collusion
├── src/
│   ├── protocol/           # Game-Master arbiter, turn channels, format-fairness
│   ├── runtime/            # GPU day/night scheduler, RAM<->VRAM swapping
│   ├── heredity/           # merge (mergekit) reproduction, genome/recipe, mutation
│   ├── organisms/          # model registry, metabolism = size, energy accounting
│   ├── world/              # grid, Donor-Game loop, turn engine
│   ├── observatory/        # logging, metrics, phylogeny, A/B
│   └── dashboard/          # Canvas aquarium + Event Feed (web)
├── config/
│   ├── models.yaml         # the 5–8 starting species (Ollama tags, sizes)
│   └── world.yaml          # energy rules, metabolism constants, 10% cap
├── data/
│   └── world.sqlite        # the live world (gitignored)
├── scripts/
│   ├── bootstrap.py        # pull models, init DB, seed population
│   └── run_world.py        # run N turns
├── tests/
├── requirements.txt
└── LICENSE
```

---

## Quick start (intended MVP flow)

> Plan, not yet shippable — these are the commands the MVP is being built toward.

```bash
# 1. Install Ollama and pull the starting species
ollama pull <model-a>   # e.g. a small + a few 7–8B models, all Q4

# 2. Python env
python -m venv .venv && source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt   # mergekit, sqlite bindings, web deps

# 3. Bootstrap the world (pull config, init SQLite, seed population)
python scripts/bootstrap.py --config config/world.yaml

# 4. Run the simulation
python scripts/run_world.py --turns 200

# 5. Open the dashboard
#    serve src/dashboard/ and watch the aquarium + Event Feed
```

---

## Get involved

This is an early-stage, design-plus-MVP project looking for **contributors and compute sponsors**. A single sponsored cluster run (Tier-Sponsor) is what unlocks full-fine-tune evolution of larger organisms — if you have idle 80GB GPUs, you can directly accelerate the science.

- **Author:** Andrii Shramko
- **GitHub:** https://github.com/AndriiShramko
- **LinkedIn:** https://www.linkedin.com/in/andrii-shramko/
- **Book a call:** https://calendar.app.google/Ff729HqGk4RpzPNDA
- **Email:** zmei116@gmail.com

If you want to watch AI organisms be born, form alliances, betray each other, and pass on their genes — star the repo, open an issue, or book a call.
