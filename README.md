# LLM-Ecosystem-Andrii-Shramko

> A digital aquarium where the "fish" are open-weight LLMs — they eat compute, die at zero energy, breed by merging their weights, mutate, and evolve. A safe sandbox for emergent multi-agent AI dynamics, a new "survival-of-the-fittest" benchmark for open models, and a live show worth watching.

![status](https://img.shields.io/badge/status-design%20%2B%20MVP-blue)
![license](https://img.shields.io/badge/license-CC%20BY%204.0%20(docs)%20%2F%20MIT%20(code)-green)
![PRs](https://img.shields.io/badge/PRs-welcome-brightgreen)
![sponsor](https://img.shields.io/badge/GPU%20sponsor-wanted-orange)

---

## 🧒 Explain it like I'm five (or to grandma)

Imagine a fish tank on a computer. But instead of fish, tiny **AI creatures** live inside it. Each creature is a real, different "brain" (an open-source AI model) — some are small and quick, some are big and slow.

To stay alive, every creature needs **food**. Here, food is **computer power** (the electricity and the chip that makes them think). If a creature runs out of energy, it **dies**. Big creatures think better but eat much more — so being huge is not a free win, it's a gamble.

When two creatures do well, they can **have a baby** — and the baby is literally made by **mixing the two parents' brains together** (we blend their actual "weights," the numbers inside the model). The baby inherits a bit of each parent, sometimes with a small random change — a **mutation**. Over many generations, the tank fills with creatures nobody designed by hand: some learn to **cooperate**, some learn to **trick** others, some become **predators**.

We watch all of this happen — births, friendships, fights, deaths — and we keep a family tree of who came from whom. It's part **nature documentary**, part **science experiment**, part **video game**. And because the whole thing runs **offline in a sandbox** with no hands in the real world, the creatures can be as wild as they want and still hurt nobody.

The big question we're chasing: *when many different AIs have to survive together, what do they naturally become — and which open model is the "fittest"?*

**In one breath (RU):** Цифровой аквариум, где живут не рыбки, а ИИ-зверьки — разные открытые модели. Они едят «еду» (вычисления), умирают при нулевой энергии и размножаются, **смешивая свои веса** (mergekit) — это настоящая наследственность. За поколения там сама собой рождается кооперация, обман и хищничество. Безопасная песочница офлайн: смотрим, кто выживает, и какая открытая модель — самая приспособленная.

---

## What this is

**LLM-Ecosystem** is an artificial-life simulation in which the *organisms are large language models*. Each organism is a distinct open-weight model (or a merged descendant of two). They live on one machine, take turns, and the entire world state lives in a database. Energy is **zero-sum** — compute is the food, and one creature's gain is the pool's loss.

**This is not:**
- **Not a hype toy** or a chatbot demo. It's a research instrument with a real evolutionary mechanism (weight inheritance), measurable phylogeny, and a safety model.
- **Not "AGI in a box"** and not a claim that anything here is conscious. These are models surviving under resource pressure, nothing more, nothing mystical.
- **Not finished.** Be honest with yourself reading this: the **9-layer architecture is fully designed**, and an **MVP is the current build target**. "Designed" ≠ "running." Where something is a plan, this README says so.

The wedge that makes it worth building is below.

---

## Why it matters / what we can learn

- **Emergent multi-agent dynamics.** When many heterogeneous AIs must compete and cooperate for finite compute, do they evolve cooperation, deception, alliances, dominance hierarchies? This is one of the open questions in AI safety, and a sandbox is the safest place to study it.
- **Open-ended evolution.** Most AI "evolution" tunes one model. Here the *population* evolves with real inheritance — a living lab for open-endedness in the tradition of Tierra/Avida, but with modern LLMs as the unit of life.
- **An AI-safety polygon.** Offline, no actuators, energy zero-sum. We can deliberately provoke collusion and reward-hacking and *measure* them — including treating detected collusion as a **lower bound**, never claiming "zero."
- **A new benchmark.** Beyond static leaderboards: *which open-weight model is the most survivable* under ecological pressure? Fitness, not just accuracy.
- **A live show.** Births, alliances, betrayals, obituaries, a phylogenetic family tree, "adopt-a-creature" streams. Science that's also genuinely fun to watch.

---

## What is genuinely novel here (the wedge)

Three things, together, are the contribution:

1. **A heterogeneous survival arena of open-weight models.** Not N copies of one model with different prompts — N genuinely *different* architectures and lineages competing in the same world.
2. **The open LLM itself as the unit of selection.** The thing that lives, dies, and reproduces is a whole model, with its metabolism scaled to its size. Size is a *trade-off*, not a cheat code.
3. **Merge as true weight heredity.** Reproduction is a **gradient-free weight merge** (mergekit, Sakana-style), where the *merge recipe is the genome* — heritable, mutatable, and phylogenetically traceable. Text/prompt tweaks are a cheap iteration channel, **not** reproduction. Only weights carry the DNA.

---

## How it works (the short version)

- **Food = compute/energy.** Every turn costs energy. Thinking, acting, and merging all draw from a shared, **zero-sum** pool.
- **Death at zero.** When a creature's energy hits 0, it dies and is removed from the world (its lineage snapshot is kept).
- **Reproduction = MERGE.** Two fit parents produce offspring by merging weights. The recipe is the genome; small random changes are mutations.
- **Metabolism scales with size.** Bigger models reason better but burn far more energy. A **"10% energy" rule** keeps giants rare — they must dramatically out-earn their upkeep or starve. **This is exactly how small creatures beat giants.**
- **Niches.** Different strategies survive in different corners of the world; there's no single winning build.
- **Predator–prey & parasitism.** Creatures can cooperate, parasitize (siphon energy), or hunt — and these strategies *emerge*, they aren't scripted.
- **The GPU is a contested resource.** Day/Night scheduling and RAM↔VRAM swapping mean access to the chip is itself something to compete over.

---

## Architecture (9 layers)

Fully designed; see **[docs/ARCHITECTURE.md](docs/ARCHITECTURE.md)** for the detail.

| # | Layer | One-liner |
|---|-------|-----------|
| 1 | **PROTOCOL** | Game-Master arbiter: 3 channels, turn order, format-fairness so no model wins on syntax. |
| 2 | **RUNTIME** | GPU Day/Night cycle, RAM↔VRAM swapping to fit big models on one card. |
| 3 | **HEREDITY** | The ladder: T1 prompt → **T2 merge = reproduction** → T3 QLoRA → T4 scaffold (OFF) → T5 sponsor full-FT. |
| 4 | **OBSERVATORY** | Logs, metrics, phylogeny tree, A/B arms — the science layer. |
| 5 | **GOALS** | 3 tiers: hardcoded survival → emergent goals → autotelic text-goal (intrinsic ≤ 10%). |
| 6 | **SAFETY** | belief-vs-ledger checks, anti-collusion as a *detected lower bound*, sandbox isolation. |
| 7 | **GAMEDESIGN** | Engagement and legibility — the "aquarium" that makes it watchable. |
| 8 | **VISUAL** | Juice, emotion bubbles, the front-of-house show. |
| 9 | **SYNTHESIS** | The stitching layer that ties all of the above into one coherent loop. |

---

## Run it yourself (MVP, ~$0 on one GPU)

The MVP is designed to run **offline, on a single consumer GPU, for ~$0** — no cloud bill, open weights only.

**What you'll need (planned stack):**
- **Python**
- A local model runner — **Ollama or vLLM**
- **SQLite** for the world state
- **mergekit** for gradient-free weight merges (the reproduction step)
- A GPU in the 24 GB class (reference rig: **RTX 4090 24 GB + 256 GB RAM**, with merges done in system RAM)

**Phase-0 / MVP scope:** the **T1 (prompt) + T2 (merge)** tiers — a small population of 7–8B 4-bit models, taking turns in a zero-sum world, dying, and breeding by merge, with a phylogeny log. QLoRA, self-rewrite, and full fine-tuning are explicitly **out of MVP**.

> **Honest status:** the code is still in the design stage — this section describes the **planned MVP**, not a finished `pip install`. Build steps land in **[docs/BUILD.md](docs/BUILD.md)** as they're implemented. Want to help make `step 1` real? See *Join or fork* below.

---

## Roadmap

- **Phase 0 — Local MVP (~$0):** single GPU, T1+T2 (prompt + merge), zero-sum energy, SQLite world, phylogeny log. *Current target.*
- **Phase 1 — Grid ecosystem:** richer world (niches, GPU Day/Night contention, predator–prey), Observatory metrics + family tree.
- **Phase 2 — Self-improvement & heredity:** T3 QLoRA "sleep" (opt-in, flagged) so creatures can adapt across generations.
- **Phase 3 — The show:** streamable aquarium, obituaries, adopt-a-creature, public phylogeny — make it watchable and shareable.
- **Phase 4 — The open planet:** anyone can plug **their own LLM + their own hardware** into a shared world. A federated ecosystem.
- **Phase 5 — Sponsor cluster:** with sponsored compute, unlock **T5 full fine-tuning** of survivors on a rented multi-GPU cluster — a different scale of evolution.

Gated, irreversible steps (T4 self-rewriting scaffold, real-money cloud, public launch under the author's brand) stay **off by default** and require an explicit decision. See the decisions registry in the docs.

---

## Join or fork

This is an open invitation. Two ways in:

- **Join:** pick a layer (PROTOCOL, RUNTIME, HEREDITY, OBSERVATORY…) or a Phase-0 task and open a PR. Read **[CONTRIBUTING.md](CONTRIBUTING.md)** to get oriented.
- **Fork:** take the idea and run your own aquarium. That's encouraged — please keep attribution (see License) and tell us what evolved. We'd love to compare worlds.

Good first contributions: a minimal turn loop, a SQLite world schema, a mergekit reproduction wrapper, an Ollama/vLLM adapter, or a phylogeny visualizer.

---

## 🖥️ We are looking for a GPU sponsor

The MVP runs on one consumer card. **Everything past Phase 2 — many-creature worlds, T3 QLoRA at scale, and especially T5 full fine-tuning — needs serious compute.**

If you (or your company) can offer **cloud credits or GPU time — Google Cloud, AWS, Azure, NVIDIA, or similar — we want to talk.** Warm intros to the right decision-makers are just as welcome as credits. In return: co-credit, visibility on a genuinely novel open-science project, and a front-row seat to the first open-weight survival arena. Details and tiers in **[SPONSORS.md](SPONSORS.md)**.

---

## Author & contact

**Andrii Shramko** — author of the idea and the 9-layer design. The core concept (open LLMs as evolving organisms, merge-as-heredity, the survival-arena benchmark) originated with and is attributed to Andrii Shramko.

- **LinkedIn:** https://www.linkedin.com/in/andrii-shramko/
- **Book a call:** https://calendar.app.google/Ff729HqGk4RpzPNDA
- **Email:** zmei116@gmail.com
- **GitHub:** https://github.com/AndriiShramko

Sponsors, collaborators, researchers, and the curious — all welcome to reach out.

---

## License

- **Ideas & documentation:** [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — free to use and adapt **with attribution to Andrii Shramko**.
- **Code:** **MIT** (applies once code lands in the repo).

Please keep the attribution if you fork or build on the concept.

---

## Documentation

- **[docs/ROADMAP.md](docs/ROADMAP.md)** — the build plan: phases, what you'll *see* in the browser each step, the ring-test, how "done" is proven.
- **[docs/ARCHITECTURE.md](docs/ARCHITECTURE.md)** — the 9 layers in full.
- **[docs/RULES.md](docs/RULES.md)** — world rules: energy, metabolism, merge/heredity, death, niches.
- **[docs/BUILD.md](docs/BUILD.md)** — set up and run the MVP (lands as it's built).
- **[CONTRIBUTING.md](CONTRIBUTING.md)** — how to join, code conventions, good first issues.
- **[SPONSORS.md](SPONSORS.md)** — GPU sponsorship: tiers, recognition, how to reach out.
