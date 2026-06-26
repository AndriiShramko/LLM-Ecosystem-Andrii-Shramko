# LLM-Ecosystem — Roadmap

> **Honest status: this is a build-in-public project. Design ≠ working.** Every phase is "done" only when you can open a URL, see the promised picture, watch the `/status` traffic-light show `N/N 🟢`, and the owner has signed off. Nothing is "done" because code exists.

**What it is:** Artificial life where organisms = different open-weight LLMs living on one machine. Reproduction = merging model weights (mergekit; the merge recipe is the "genome"). Metabolism is proportional to model size; an organism dies when its energy hits zero. A digital aquarium of AI critters — for everyone — with a deep lab view for AI engineers.

## Stack (2026)
Next.js 16 (React 19.2) · PixiJS v8 (WebGPU 2D aquarium) · FastAPI + Server-Sent Events bridge · Redis Streams event bus · Zustand + TanStack Query · uPlot + visx charts · Playwright (visual ring-tests) · Docker Compose. 3D Gaussian-Splatting view (Spark.js 2.0 + React Three Fiber) is deferred to a later, optional attraction phase.

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
| **3 — Breeding = MERGE (T2)** | mergekit merges two models into a child | Two fish touch → a child appears with a real genome recipe; clickable family tree; **"Re-run merge" reproduces the identical genome on screen** | ⬜ planned |
| **4 — Niches & predator–prey** | Ecosystem dynamics + full dashboards; zero-sum energy | Niche zones, chases, ❤️ Follow, survival leaderboard, live charts; **"Total energy: 1000.0 (never changes)" always on screen** | ⬜ planned |
| **5 — Social loop + tweets** | Shareable cards, hero-of-the-day, point bets, approval-gated tweets | "Share" → a real PNG of your critter; bets resolve next morning; after you approve in Telegram, **you click the real tweet URL** | ⬜ planned |
| **6 — Dev→Prod review gate** | Propose → Issue → PR → CI ring-test → joint decision | "Propose" button files a GitHub Issue; PRs get an auto ring-test GIF; "Ship it? Yes/No" | ⬜ planned |
| **7 — Server + public URL** | Runs on a real server; **read-only** public URL; capped unattended auto-tweets | Open it from your phone (read-only — nobody can poke the GPU/merge); auto-posts on X | ⬜ planned |

T3 is deferred. Safety tiers T4/T5 ship **OFF, signature-gated** — shown as locked 🔒 cards until the owner explicitly approves.

**Tweets are approval-gated** in Telegram through Phase 6 (including dev milestones); fully unattended posting — capped by `MAX_TWEETS_PER_DAY` and a `TWEETS_ENABLED` kill-switch — only arrives in Phase 7.

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
