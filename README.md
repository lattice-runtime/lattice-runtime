<div align="center">

# Lattice Runtime

### Don't adopt a vendor.

**Model-agnostic managed agent infrastructure.**
**Run anywhere. Govern everything. Own your data.**

[![Early Access](https://img.shields.io/badge/Early_Access-Open-0e7490?style=flat-square)](#get-early-access)
[![License](https://img.shields.io/badge/License-Apache_2.0-blue?style=flat-square)](#)
[![Go](https://img.shields.io/badge/Go-1.24+-00ADD8?style=flat-square)](#)
[![Tests](https://img.shields.io/badge/Tests-273_passing-8b5cf6?style=flat-square)](#)

[Website](https://lattice-runtime.github.io/architecture) &middot; [Architecture](https://lattice-runtime.github.io/architecture/architecture) &middot; [Blog](https://lattice-runtime.github.io/architecture/blog)

</div>

---

## The Problem

Managed agent infrastructure is here. But it's closed, single-vendor, and cloud-only.

When your agent infrastructure is locked to one provider, your entire system becomes a pet tethered to one API, one pricing model, one uptime SLA. Today's best model will not be tomorrow's best model. The team that bets on one provider rewrites everything when the leaderboard shifts.

## The Fix: Decouple the Brain from the Model

Lattice Runtime virtualizes the model interface. The brain calls `generate(messages, tools)` &mdash; it does not know if Claude, GPT, Gemini, or a local Ollama instance is behind it.

```
                   Lattice Runtime
  ┌──────────────┬──────────────┬──────────────────┐
  │    Brain     │   Session    │      Hands       │
  │  (harness)   │  (durable)   │   (runtimes)     │
  ├──────────────┼──────────────┼──────────────────┤
  │ Agent loop   │ Append-only  │ Local            │
  │ Any model    │ event log    │ Git Worktree     │
  │ Context eng  │ Crash-safe   │ SSH / Remote     │
  │ Compaction   │ Queryable    │ Docker           │
  │ Orchestrator │ Self-healing │ Devcontainer     │
  │ Sub-agents   │ Portable     │ Lattice SSH      │
  └──────┬───────┴──────┬───────┴────────┬─────────┘
         │              │                │
    ┌────┴────┐   ┌─────┴─────┐    ┌────┴────┐
    │ Claude  │   │ Temporal  │    │ Execute │
    │ GPT     │   │ Durable   │    │ Sandbox │
    │ Gemini  │   │ Execution │    │ Isolate │
    │ Ollama  │   │ Recovery  │    │ Secure  │
    │ Any LLM │   │ Replay    │    │         │
    └─────────┘   └───────────┘    └─────────┘
```

**Three interfaces. Each can fail independently. Each can be swapped.**

- `generate(messages, tools) → response` &mdash; the brain doesn't know which model
- `getEvents(from, limit) → stream` &mdash; durable session, not a context window
- `execute(name, input) → string` &mdash; same interface for any runtime

## How It Compares

| | Vendor-Locked | Lattice Runtime |
|---|---|---|
| **Model** | One provider only | Any model, any provider |
| **Hosting** | Provider cloud | Your cloud, your machine, or ours |
| **Data** | Provider servers | Wherever you run it |
| **Multi-agent** | Single-model | Mix models per agent role |
| **Governance** | Not built-in | 5 gates, budgets, crypto audit |
| **Runtimes** | Cloud container | 6 backends (local &rarr; K8s) |
| **Crash recovery** | Provider-dependent | Resume with any model |
| **Session** | Provider-managed | Durable, portable, yours |
| **Credentials** | Shared environment | Forwarded, never exposed |
| **Audit trail** | Basic logging | SHA-256 hash chain, tamper-evident |
| **Provisioning** | Upfront container | Lazy, on first tool call |

## Five Governance Gates

Every agent action passes through five gates before it executes. Policy violations are structurally impossible.

```
  Request → IDENTITY → AUTHORIZATION → CONSTRAINTS → EXECUTE → AUDIT
               │            │              │             │         │
           OAuth2/SAML   RBAC+ABAC     Budgets,      Temporal   SHA-256
           mTLS/API key  Rego→SQL      PII filter,   Durable    hash chain
                                       Model lock    Workflow    Immutable
```

## What's Inside

- **Single Go binary** (`latticed`) with embedded Temporal v1.30
- **PostgreSQL** &mdash; 50+ tables, 424 migrations, SQLC generated
- **273 tests** passing
- **WireGuard + DERP** zero-trust mesh networking
- **6 runtime backends** &mdash; Local, Worktree, SSH, Docker, Devcontainer, Lattice SSH
- **Cryptographic audit** &mdash; SHA-256 hash-chained, tamper-evident log

## Get Early Access

We're a two-person team rolling out access in phases.

**To get on our radar:**

1. **Star this repo** &mdash; we track interest and reach out to stargazers first
2. **[Open an issue](../../issues/new?template=early-access.md)** &mdash; tell us your use case so we know who to prioritize
3. **Watch for the invite** &mdash; we'll reach out in waves

Priority goes to teams running agent workloads who need self-hosted, governed infrastructure.

---

<div align="center">

**The abstraction outlasts the provider.**

[Website](https://lattice-runtime.github.io/architecture) &middot; [Full Architecture](https://lattice-runtime.github.io/architecture/architecture) &middot; [Engineering Blog](https://lattice-runtime.github.io/architecture/blog)

</div>
