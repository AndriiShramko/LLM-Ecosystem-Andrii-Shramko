# Security Policy

## Scope

LLM-Ecosystem is an **offline, sandboxed** artificial-life simulation. It is designed to have **no real-world actuators**: organisms can only emit in-world actions/observations and short messages, energy is zero-sum, and the world runs locally. A core safety property of the project is that this containment holds.

## What we especially want to hear about

- **Sandbox escape:** any way an organism (or a contributed model/adapter) could cause real-world side effects — network calls, filesystem writes outside the sandbox, code execution, resource exhaustion of the host beyond its energy budget.
- **Safety-gate bypass:** any way to enable a gated capability (self-modifying scaffold, real-money/cloud paths) without the explicit operator sign-off.
- **Prompt-injection / channel abuse:** an organism's `speech`/`goal` text being treated as an instruction by another organism, the narrator, or the public stream.
- **Secret/credential leakage** in the repo or build.

## How to report

Please report privately to **zmei116@gmail.com** (subject: `SECURITY - LLM-Ecosystem`) or via a GitHub private security advisory. Do **not** open a public issue for a security vulnerability.

We aim to acknowledge reports within a few days. Responsible disclosure is appreciated and credited (with your consent).

## Supported versions

This is an early-stage project. Security fixes are applied to the latest release on the `main`/`dev` line.
