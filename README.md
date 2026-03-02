# Rhea Tutorials

**Build a multi-model AI verification system from scratch.** Desktop. CLI. Cloud. Phone. All one system.

These lessons walk you through the architecture decisions behind Rhea — not as documentation, but as a progression. Each lesson builds on the last. By the end, you understand why every layer exists and how they switch seamlessly.

## The Architecture Question

> "Why do you need Fly.io AND Google Cloud? Why not just run Python locally?"

That's the question these tutorials answer. The short version:

```
Your laptop  ──→  Local tribunal (:8400)  ──→  AI providers (Gemini, Claude, GPT)
     │                    │
     │              Fly.io mirrors this exact server
     │              so your phone can reach it too
     │                    │
     └── Same API ────────┘── Same API ──→ iOS app, macOS app, CLI, web
```

**One server, many surfaces.** The cloud isn't a separate product — it's the same `tribunal_api.py` running somewhere your phone can reach.

## Lessons

### Part 1: The Core (run it locally, understand it fully)

| # | Lesson | What you build | What you learn |
|---|--------|---------------|----------------|
| 01 | [Your first tribunal](lessons/01-first-tribunal.md) | A Python script that asks 3 models the same question | Why single-model answers are unreliable |
| 02 | [Consensus scoring](lessons/02-consensus.md) | Agreement detection across model responses | How to measure when models converge vs diverge |
| 03 | [The sceptic](lessons/03-sceptic.md) | An adversarial model that attacks surviving claims | Why debate is a better alignment tool than RLHF |
| 04 | [Proof persistence](lessons/04-proofs.md) | SQLite proof chain (Aletheia) | Why verified claims should be immutable and citable |
| 05 | [The API layer](lessons/05-api.md) | FastAPI wrapping your tribunal | How one Python file becomes a multi-surface backend |

### Part 2: Surfaces (same server, every device)

| # | Lesson | What you build | What you learn |
|---|--------|---------------|----------------|
| 06 | [CLI interface](lessons/06-cli.md) | Rust binary that talks to localhost:8400 | Why compiled CLIs beat Python scripts for daily use |
| 07 | [Web frontend](lessons/07-web.md) | Next.js app consuming the API | How the web layer adds zero new logic |
| 08 | [iOS app](lessons/08-ios.md) | SwiftUI client with Keychain auth | Same API, native feel, offline-capable |
| 09 | [macOS operations centre](lessons/09-macos.md) | 12-pane SwiftUI window (Play) | Why monitoring needs a native app, not a dashboard |

### Part 3: Cloud (why and how)

| # | Lesson | What you build | What you learn |
|---|--------|---------------|----------------|
| 10 | [Deploy to Fly.io](lessons/10-fly.md) | Your tribunal running at `yourname.fly.dev` | Cloud = same binary, reachable address. That's it. |
| 11 | [Auth and identity](lessons/11-auth.md) | JWT + OAuth (Google, Apple, Microsoft) | Why auth lives on the server, not the client |
| 12 | [Billing and credits](lessons/12-billing.md) | Credit ledger, patron keys, crypto | How NPO economics work: 95% credits, 5% impact |
| 13 | [Multi-provider resilience](lessons/13-providers.md) | Bridge across 6+ providers, 31 models | Why vendor lock-in is an existential risk |

### Part 4: The Switch (everything connects)

| # | Lesson | What you build | What you learn |
|---|--------|---------------|----------------|
| 14 | [Desktop ↔ Cloud](lessons/14-desktop-cloud.md) | Seamless switch between localhost and fly.dev | One config change, zero code changes |
| 15 | [Phone ↔ Desktop](lessons/15-phone-desktop.md) | Start a tribunal on phone, review on Mac | Why the API-first architecture enables this for free |
| 16 | [The governor](lessons/16-governor.md) | Token budgets, cost tracking, provider routing | How to run a multi-model system without going broke |
| 17 | [Memory across sessions](lessons/17-memory.md) | Persistent agent memory (rhea-memory) | Why LLMs need external memory to be useful |

## Prerequisites

- Python 3.11+
- At least one AI API key (Gemini free tier works)
- Xcode 15+ (for iOS/macOS lessons)
- `brew install flyctl` (for cloud lessons)

## Philosophy

These are not copy-paste tutorials. Each lesson explains the **decision** that created the code — what we tried, why it failed, what survived. The architecture is a fossil record of solved problems.

---

Part of [TimeLabs NPO](https://github.com/timelabs-npo). All code MIT licensed.
