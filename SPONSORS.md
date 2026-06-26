# Sponsor the GPU cluster — help digital life evolve

> **TL;DR:** This open-science project runs **for ~$0 on one consumer GPU today** — and that already produces real evolution. A GPU sponsor doesn't *rescue* the project; it **raises the ceiling**: bigger organisms, full fine-tuning of survivors, longer runs, a reproducible benchmark, and the "open planet" where the whole world can join. If you (or your company) can offer **cloud credits, GPU time, or a warm intro to the right decision-maker**, let's talk.

---

## Who we are & why this needs compute

LLM-Ecosystem is an independent, open-source research project by **Andrii Shramko**: an artificial-life world where the organisms are *different open-weight LLMs* that survive, die, and **reproduce by merging their weights**. It is simultaneously real science (emergent cooperation/deception, open-ended evolution, an AI-safety sandbox), a new "survival-of-the-fittest" benchmark for open models, and a genuinely watchable live show.

It is built by a solo founder on a low budget. The **floor** is already covered by a single RTX 4090. The **ceiling** — the experiments that would make this matter to the whole field — needs more silicon than one person can buy.

## Honest: what runs free today vs what a sponsor unlocks

We will never overstate this. **On one RTX 4090 + 256 GB RAM, the project already shows genuine evolution:** weight-level inheritance via gradient-free **merge** (full-rank, no training cost), Lamarckian adaptation via LoRA, plus selection, mutation, and recombination over a population. That is a *bona fide* evolutionary system, not a toy.

What a sponsor **adds** is the upper trajectory:
- **Full fine-tuning (T5)** of survivors — the only tier that can grow *genuinely new* capabilities from scratch, not just recombine existing ones.
- **Larger organisms** (13B–70B) as apex species instead of only ≤8B.
- **Bigger populations × more generations × richer environments** — the real levers of open-ended novelty.
- **Long, reproducible benchmark runs** that labs can cite.
- **The "open planet"** at scale — many people's models + compute in one shared world.

So: sponsorship **accelerates and scales**; it is not a prerequisite for the science to begin.

## What we actually need (compute tiers)

| Tier | Hardware | Unlocks | Rough cost |
|---|---|---|---|
| **Local (have it)** | 1× RTX 4090 24 GB + 256 GB RAM | MVP, merge-reproduction, ≤8B organisms, the whole "floor" | ~$0 (owned) |
| **Single big card** | 1× RTX 6000 Pro / H100 **96 GB** | **Full fine-tune of organisms ≤ ~8B**; one ~70B "rare giant" in 4-bit for inference | ~$8–9k card, or cloud credits |
| **Cluster node** | **8× H100/H200 80 GB** (640 GB) | **Full fine-tune up to ~70B** + large concurrent populations + the open planet at scale | ~$25–32/hr on-demand cloud, less on spot |

Notes for the technically minded: full fine-tuning costs ~16–18 bytes/parameter (weights + grads + Adam states + master weights) plus activations. A single 96 GB card does full-FT cleanly up to ~3B, and ~7–8B with 8-bit Adam + gradient checkpointing + offload; 13B needs RAM/NVMe offload; 32B–70B need multi-GPU (FSDP/ZeRO-3). Marketing claims that "96 GB runs 70B" refer to **inference or LoRA**, not full fine-tuning.

## Who can help (and how)

We're looking for one of three things, in order of ease:

1. **Cloud-credit programs** — these exist precisely for projects like this, exchanging credits for visibility:
   - **NVIDIA Inception**
   - **Google for Startups Cloud** / Google Cloud research credits
   - **AWS Activate** / AWS Cloud Credit for Research
   - **Microsoft for Startups Founders Hub** / Azure research credits
   - **Lambda**, **CoreWeave**, **Oracle for Startups** research/credit programs
2. **A warm introduction** to a decision-maker who runs developer-relations, research-credits, or open-source / AI-safety programs at **Google, Amazon, Microsoft, or NVIDIA** — an intro is often worth more than a cold application. **If you know the right person, please connect us.**
3. **Direct GPU time** — idle cluster hours, an academic allocation, or a donated node.

We ask for something **specific** — "X GPU-hours over Y weeks to run experiment Z" — not "please help." This is a value exchange, not a donation drive.

## What a sponsor gets

- **Logo & credit** in the README, on the live dashboard/stream, and in every release ("Powered by …").
- **Citation/acknowledgement** in any preprint or public benchmark that results.
- **Front-row seat** to the first open-weight survival arena, and a co-branded post/case-study about running it on your infrastructure.
- **Open-source & AI-safety goodwill** — this is public, reproducible, and safety-relevant work.

(Every concrete sponsorship deal is reviewed and signed off by Andrii personally — co-branding and attribution commitments are his call.)

## Reach out

- **LinkedIn:** https://www.linkedin.com/in/andrii-shramko/
- **Book a call:** https://calendar.app.google/Ff729HqGk4RpzPNDA
- **Email:** zmei116@gmail.com
- **GitHub:** https://github.com/AndriiShramko

If you can give compute, credits, or an intro to someone who can — **a single message could move this from one GPU to a planet.** Thank you.

— *Andrii Shramko, author of the LLM-Ecosystem concept and architecture.*
