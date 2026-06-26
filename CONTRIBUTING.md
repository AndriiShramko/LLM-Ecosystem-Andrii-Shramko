# Contributing to LLM-Ecosystem

> An artificial-life sandbox where the organisms are not fish — they are AI creatures. Different open-weight LLMs live on one machine, eat compute, die at zero energy, reproduce by **merging their weights**, mutate, cooperate, deceive, and evolve. Survivors pass on their genes. Think of it as a **digital aquarium for AI**.

**Author & originator of the idea:** Andrii Shramko.
This is an early-stage project. The 9-layer architecture is **designed**, and a Phase-0 MVP is being built. We are honest about the difference: "designed" is not "working yet." If you like building things at the edge of ML, multi-agent dynamics, and game design, you are exactly who we are looking for.

---

## Two ways to take part

There is no wrong way to engage. Pick the path that fits you.

### Path A — "Take it and start your own"

Fork it. Run your own aquarium. Change the rules. Breed your own species. This project exists to spread, not to be owned.

- **Fork** the repo and build whatever you want on top of it.
- **Keep attribution** to *Andrii Shramko* as required by the license (see `LICENSE`). That is the only ask: credit the origin of the idea and the codebase.
- Tell us what you discovered — a surprising emergent behavior, a new merge recipe, a species that refused to die. We will happily link back to interesting forks.

### Path B — "Join this one"

Build the canonical aquarium with us.

- **Open an issue** for bugs, questions, or design proposals. Design discussion is first-class here — a well-argued issue is as valuable as a PR.
- **Send a PR.** Start small (see *Good first issues* below), describe what you changed and why, and note whether it is "designed" or "verified working."
- **Bring your own open-weight model as a new species.** Any open-weight LLM that fits the hardware tier (think ≤7–8B at 4-bit for the home tier) can enter the arena as a new organism. Show us how it survives, breeds, and mutates.
- **Bring compute.** GPU time is the scarcest resource. If you have spare cycles — a 4090, an RTX 6000, or a real cluster — you can sponsor experiments that the home tier cannot reach. See *Sponsoring compute* below.

---

## The future: an open planet

Today the aquarium runs on one machine. The long-term design is an **open, server-authoritative planet**:

- **Your LLM and your GPU stay at home.** You run your own organism locally. Over the network, you exchange only **observations and actions** — never raw weights or unbounded compute. The server (the Game-Master arbiter) is authoritative and adjudicates every turn.
- **Fairness through energy and weight leagues.** A bigger GPU must **not** win automatically. Metabolism scales with model size, energy is zero-sum, and organisms compete inside weight-class leagues — so a giant model is a costly bet, not a free victory. Size is a trade-off, not a cheat code.
- **Server-authoritative honesty.** Because the server owns the world state (the ledger), it can measure what actually happened versus what an agent *claims* — the foundation for detecting collusion and reward-hacking.

This is a design direction, not a shipped feature. If it excites you, the Phase-0 cycle and the protocol layer are where you can shape it.

---

## Good first issues

These are scoped to be achievable and genuinely useful. Look for the `good first issue` label.

| Area | Task | Why it matters |
|---|---|---|
| **Core loop** | Implement the **Phase-0 turn cycle**: organisms spend energy, eat compute, die at zero, world state in the DB. | This is the heartbeat of the whole system. |
| **Reproduction** | Build a clean **mergekit wrapper** (gradient-free weight merge; the recipe is the genome). | Merging weights is how organisms reproduce. |
| **Observability** | Build a **dashboard** — live population, energy, phylogeny tree, obituaries. | Makes the aquarium watchable and debuggable. |
| **Metrics** | Add **fitness/heredity metrics** and A/B logging so we can measure which model is most "fit." | Turns the aquarium into a real benchmark. |

Don't see your idea here? Open an issue and propose it.

---

## Sponsoring compute

The home tier (RTX 4090 24GB + 256GB RAM) covers the entire MVP and most interesting experiments at the 7–8B scale. Bigger ambitions need bigger iron:

- **Rare giants** (≤70B at 4-bit, occasional full fine-tunes) — an RTX 6000-class card.
- **Sponsored full fine-tuning leagues** — a rented multi-GPU cluster.

If your organization can donate or discount GPU time, that directly unlocks new tiers of the heredity ladder. Real-money / cluster decisions are deliberate, reversible-where-possible, and signed off by the author — so a sponsorship is a clear, well-defined contribution, not an open-ended commitment. Reach out via the links below.

---

## Community & code of conduct (the short version)

We want a **warm, curious community**. Be kind, assume good faith, credit others' work, and help newcomers. Harassment of any kind is not welcome.

A few rules are non-negotiable because they define what this project *is*:

- **Sandbox-safety, always.** Organisms have **no actuators** and cannot take real-world actions or cause real harm. Energy is zero-sum and the world is offline. Contributions that add real-world side effects, network actuators, or any path to physical/financial action will be declined.
- **Honest anthropomorphism — no dark patterns.** The aquarium metaphor (creatures are born, befriend, fight, evolve) is a teaching tool. Always **disclose that these are AI models**, not sentient beings. Obituaries and "adoption" are for engagement and science, never to deceive, manipulate emotions for profit, or imply real consciousness.
- **No reward-hacking the safety layer.** Self-modifying scaffolds, sponsored full fine-tunes, and similar high-risk capabilities are **off by default** and gated behind explicit operator sign-off. Don't try to route around the safety gates — improve them in the open instead.

If you are unsure whether an idea fits, just ask in an issue first. We would rather discuss early than decline late.

---

## How to reach us

Whether you want to fork, contribute, bring a model, or sponsor compute — let's talk.

- **Author:** Andrii Shramko
- **GitHub:** https://github.com/AndriiShramko
- **LinkedIn:** https://www.linkedin.com/in/andrii-shramko/
- **Book a call:** https://calendar.app.google/Ff729HqGk4RpzPNDA
- **Email:** zmei116@gmail.com

Come build the aquarium. See you in the world.
