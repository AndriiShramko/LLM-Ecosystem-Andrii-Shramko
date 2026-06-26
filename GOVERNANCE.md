# Governance

This document describes how decisions are made in **LLM-Ecosystem** and how code reaches production.

## Roles

- **Lead maintainer & author of the idea:** **Andrii Shramko**. Final say on direction, releases, branding, licensing, and anything irreversible (real-money spend, public launches under the project's name, safety-gate changes).
- **Maintainers:** trusted contributors who can review and merge PRs into the development branch. Added by the lead maintainer based on a track record of good contributions.
- **Contributors:** anyone who opens an issue or PR. You don't need permission to start — see [CONTRIBUTING.md](CONTRIBUTING.md).

## How decisions are made

- **Reversible, low-cost, $0:** decided in the open by maintainers, fast. Most design and code choices live here.
- **Irreversible / money / brand / safety:** decided by the lead maintainer. These are explicitly enumerated as "operator gates" and stay **off by default** until signed off:
  - real-money spend or rented compute clusters,
  - enabling self-modifying scaffolds (T4),
  - sponsored full fine-tuning (T5),
  - public launch / branding / labels,
  - license changes.

A registry of these defaults and who decides them is maintained with the project design notes.

## Dev → Production: the promotion gate

Not every merged contribution runs in the **canonical / production** simulation automatically. We separate *experiment* from *production* on purpose:

1. **`dev` (default branch):** where contributions land. Merged after review + green CI + the loop-test for the relevant feature passing.
2. **Staging / canary:** a feature runs against the ring/loop test suite and a short canary simulation. Behavior is observed, not assumed.
3. **Production / canonical run:** code is promoted to the reproducible, public-facing canonical simulation **only after a joint decision** (maintainers propose, lead maintainer signs off). The canonical run must stay reproducible (fixed seeds, no audience interference) and sandbox-safe.

This is how we "decide together what to ship to production and what not to" — proposals are public, the bar is *demonstrated* (tests + canary), and the final promotion is an explicit, recorded decision.

## Versioning & rollback

We follow [Semantic Versioning](https://semver.org/) and keep a [CHANGELOG.md](CHANGELOG.md). Every production promotion is a tagged release so we can roll back instantly to the last known-good version if something breaks.

## Changing this document

Open a PR. Governance changes are decided by the lead maintainer.
