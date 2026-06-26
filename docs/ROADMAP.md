# LLM-Ecosystem — Roadmap

> **Honest status: this is a build-in-public project. Design ≠ working.** Every phase is "done" only when you can open a URL, see the promised picture, watch the `/status` traffic-light show `N/N 🟢`, and the owner has signed off. Nothing is "done" because code exists.

> ✅ **2026-06-26 — plan v2, vetted.** This roadmap was stress-tested by a panel of domain specialists + adversarial critics (~55 corrections applied), and the key decisions are now made: reproduction uses **full model weights** on **small models** (1.5–3B) to first prove the system is viable on a single RTX 4090; announcements go to a **Telegram channel (not X/Twitter)**; the world runs **episodically on local hardware** for now (no cloud cluster). A dedicated **"proof of selection"** gate (Phase 3c) must pass *before* the showcase is built. Phases are still a direction, not a contract — the build itself hasn't started.

**What it is:** Artificial life where organisms = different open-weight LLMs living on one machine. Reproduction = merging model weights (mergekit; the merge recipe is the "genome"). Metabolism is proportional to model size; an organism dies when its energy hits zero. A digital aquarium of AI critters — for everyone — with a deep lab view for AI engineers.

## Stack (2026, revised after vetting)
**Vite + React** (SPA; lighter than Next.js, and the public Phase-7 view is a separate static build for a smaller attack surface) · **PixiJS v8 with WebGL2 by default** (WebGPU is an opt-in toggle, not the default — it can render a black first frame on some drivers) · vanilla PixiJS (not `@pixi/react`) for the aquarium · **FastAPI + Server-Sent Events** bridge · Redis Streams (added once the engine becomes its own container, not on day one) · Zustand for live state · uPlot charts · Playwright (visual ring-tests) · Docker Compose (GPU via NVIDIA Container Toolkit; on Windows this means the WSL2 backend). 3D Gaussian-Splatting (Spark.js 2.0) stays out of v1.0 — an optional later attraction, not a promise.

> Honest cost note: the core runs locally at **near-zero cash cost** — announcements use a **free Telegram bot** (not the paid X API), and the only real running cost is **electricity** while the machine is on. No cloud cluster is needed for the viability proof.

## Two views, one app
A single toggle **[👁 Spectator | 🔬 Expert]** switches *depth*, not data:
- **Spectator** — names, emoji avatars, health bars, leaderboards, "adopt & root for a critter", shareable hero/obituary cards.
- **Expert** — `model_id`, parameter count, merge-recipe genome (YAML), energy ledger, fitness metrics, tokens/s & latency, phylogenetic tree, GPU day/night load.

The toggle is URL-state (`?view=spectator|expert&org=…`) so any link reopens the exact critter in the exact mode.

## Phases & what you'll see

| Phase | Goal | What you SEE in the browser | Status |
|---|---|---|---|
| **0 — Lights on** | Shell + empty aquarium + view toggle + `/status` | Site opens, toggle flips views, traffic-light "4/4 🟢" | ⬜ planned |
| **1 — Critters swim** | PixiJS aquarium on mock data | 5 fish swim, energy bars drain, hover shows a card, badge 🟫 DEMO | ⬜ planned |
| **2a — GPU breathes** | A real model runs in the GPU container (plumbing proof) | `/status` shows "GPU detected: RTX 4090 24GB" read from inside the container; a real model answers a prompt; tokens/s + VRAM in Expert | ⬜ planned |
| **2b — Metabolism + death (T1)** | Energy drains ∝ model size; death at zero | Badge flips 🟩 LIVE; fish slims as it infers; "Feed" button; death + obituary; **energy shown side-by-side (GUI vs backend) with a MATCH ✅ badge on `/status`** | ⬜ planned |
| **3 — Breeding = MERGE** | mergekit merges two models' **full weights** into a child | Two fish touch → a child appears with a real genome recipe; clickable family tree; **"Re-run merge" reproduces the identical recipe-hash on screen** | ⬜ planned |
| **3c — Proof of selection** ⭐ | *Does merge under selection actually improve fitness — or is it just a slideshow of merges?* An A/B on a cheap, verifiable benchmark | **Two charts side by side:** "with selection" climbs, "random merge" stays flat. A **pre-declared number `A−B ≥ X`** on a held-out task slice decides PASS/FAIL. If it fails, the project honestly becomes a "genealogy zoo," not "life." **This gate comes *before* the showcase.** | ⬜ planned |
| **4 — Niches & predator–prey** | Ecosystem dynamics + full dashboards; zero-sum energy | Niche zones, chases, ❤️ Follow, survival leaderboard, live charts; **"Total energy: 1000.0 (never changes)" always on screen** | ⬜ planned |
| **5 — Social loop + Telegram announcements** | Shareable cards, hero-of-the-day, point bets, approval-gated posts | "Share" → a real PNG of your critter; after you approve in **Telegram**, the post goes to the project's **Telegram channel** (not X/Twitter) | ⬜ planned |
| **6 — Dev→Prod review gate** | Propose → Issue → PR → CI ring-test → joint decision | "Propose" files a GitHub Issue; PRs get an auto ring-test GIF; promotion to production needs a **GitHub Environment approval** the owner clicks | ⬜ planned |
| **7 — Server + public URL** | Runs on a server; **read-only** public URL; capped, moderated **Telegram** announcements | Open it from your phone (read-only — nobody can poke the GPU/merge); auto-announcements go to **Telegram** | ⬜ planned |

T3 is deferred. Safety tiers T4/T5 ship **OFF, signature-gated** — shown as locked 🔒 cards until the owner explicitly approves.

**Announcements go to a Telegram channel (not X/Twitter)** and are approval-gated through Phase 6 (including dev milestones). Fully unattended posting — capped by a daily quota, an `ANNOUNCE_ENABLED` kill-switch, and **per-post output moderation that defaults to HOLD (never auto-publish) on a skipped check** — only arrives in Phase 7.

## The ring-test (how "done" is proven, not claimed)
Each phase runs a **ring/loop**: `birth → metabolism → inference → merge → tree → death → event on the bus → SSE → PixiJS repaint`, looping until *every* checkpoint is green on a human-readable `/status` page. Two checkpoints exist purely to catch hallucinated progress: **the same number is fetched by two independent code paths and shown side-by-side on `/status` with a MATCH ✅ badge** (not asserted by the agent — shown on screen for you), and **stopping the data-source container (`api`, then `inference`) makes the picture go 🟥 STALE within seconds**. CI attaches a before/after GIF to every PR.

## Versioning & rollback
`main` is always green. Each shipped phase = an annotated git tag (`v0.0-shell` … `v1.0`) + a GitHub Release with video, screenshots, and the Playwright report. Roll back = `git checkout <tag> && docker compose up -d`.

## How to contribute (by phase)
1. **Pick the current phase** (the first ⬜ row above — earlier phases must be owner-confirmed first).
2. **Run it locally:** `git clone` → `docker compose up` → open `http://localhost:3000`. GPU work lives only in the `inference` container (NVIDIA Container Toolkit); the rest runs anywhere.
3. **Spot a problem in Expert view → "Propose":** it auto-files a GitHub Issue with the metrics snapshot, organism IDs, merge recipe, and logs.
4. **Open a PR.** CI runs the phase's ring-test in an isolated sandbox and posts pass/fail + a GIF to your PR.
5. **Joint review gate:** anything touching life/death/safety rules (SAFETY / GOALS / HEREDITY layers) or enabling tiers T3/T4/T5 needs explicit owner approval before merge to `main`. Everything merged into `main` deploys to the production aquarium; until then it lives in a `staging` aquarium with a STAGING badge.

**License:** MIT (code) + CC BY 4.0 (content/data). Author: Andrii Shramko.
