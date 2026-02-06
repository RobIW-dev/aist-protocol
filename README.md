# AIST: AI State Transfer Protocol

**Your AI forgets everything between sessions. Here's an open protocol that fixes it.**

AIST is a lightweight, model-agnostic protocol for preserving project state across AI sessions. It compresses 40,000+ tokens of conversation into ~950 tokens of structured state — a 60x compression ratio — so your next session picks up exactly where you left off.

No vendor lock-in. No tooling required. Works on any LLM that reads plain text.

---

## The Problem

Every AI session starts from zero. Your AI doesn't remember the architectural decisions you made yesterday, the three approaches you rejected last week, or the calibration refinements that took 40 minutes of real conversation.

The usual workarounds don't work:

- **Copy-paste context** grows with every session until it fills the context window
- **RAG** retrieves fragments, not structured project state
- **Platform memory** captures personality preferences, not the decision tree of an active project
- **Starting over** means re-deriving what you already know — and losing what you forgot to mention

AIST solves this with structured compression. One handoff document. Constant size. Session 20 costs the same 950 tokens as session 2.

## Why This Matters

### 1. Your AI forgets everything

A 40-minute conversation about architecture decisions, rejected approaches, and hard-won calibration produces ~40,000 tokens of context. Tomorrow, all of that is gone. You'll re-explain, re-decide, re-calibrate — or worse, you'll forget to mention something critical and the AI will re-derive a solution you already rejected.

### 2. The least privileged benefit the most

A Pro user with 200K context loses 0.5% to an AIST handoff. Barely noticeable. But someone on a free tier (8K context), a local model (4K-32K), or an API budget in the developing world — for them, AIST is the difference between multi-session projects being possible or not.

Without AIST, project recaps grow with every session until they consume the context window. On a 4K local model, that's death after 2-3 sessions. With AIST, the handoff stays constant forever.

### 3. Small models can now do sustained work

AIST drops the minimum viable context for multi-session project work from ~32K to ~4K. A $200 used GPU running a 7B model can maintain project continuity that previously required a $20/month subscription. For students, hobbyists, and developers in countries where $20/month is significant — this is access, not convenience.

## Quick Start

### 1. Generate a handoff

At the end of any AI session, say:

> "Generate an AIST handoff for this session."

The AI will produce a structured document like this:

```
@AIST/5.0

§HEADER
project: my-project | session: 2026-02-06T14:00:00Z

§ESSENCE
Built authentication system. OAuth2 + PKCE chosen over session tokens.
JWT refresh rotation implemented. Rate limiting designed but not built.

§MEMORY
+stack: FastAPI, PostgreSQL 16, Redis for sessions
+auth: OAuth2 + PKCE, JWT with 15min/7day rotation
+deployed: staging only, production blocked on rate limiting
!warning: Redis connection pooling not tested under load

§DECISIONS
[D1] 2026-02-06 "oauth2-pkce-over-sessions"
     why: Stateless, scales horizontally, mobile-friendly
     rejected: session-tokens (sticky sessions, doesn't scale)
     revisit-when: If we add SSR that needs server-side state

§THREADS
[T1] rate-limiting status:designed effort:3h
     next: Implement sliding window in Redis

§HANDOFF
to: next-session | focus: T1 rate limiting
```

### 2. Start the next session

Paste the handoff at the beginning of your next session:

> "Here's where we left off: [paste handoff]"

The AI resumes with full context — decisions, rejected alternatives, warnings, and next steps — in ~200 tokens instead of re-explaining everything from scratch.

### 3. That's it

No tools to install. No API keys. No platform dependency. AIST is plain text that any LLM can read and generate.

## What AIST Captures

| Section | What it preserves | When to use |
|---------|-------------------|-------------|
| **§ESSENCE** | One-breath project summary | Always |
| **§MEMORY** | Key facts, warnings, deprecations | Always |
| **§DECISIONS** | Choices + what was rejected and why | Decisions were made |
| **§THREADS** | Active work streams + next steps | Tasks exist |
| **§CALIBRATION** | Before/after refinement pairs | Creative/voice/design work |
| **§HEURISTICS** | Discovered judgment rules (if/then/test) | Rules emerged from work |
| **§WORKING-STYLE** | How you collaborate | Multi-session projects |
| **§IMPLEMENTATION** | Bug fixes and solutions | Fixes were discovered |
| **§IDEAS** | Ideas with archive sync status | Ideas emerged |
| **§EVOLUTION** | How scope changed and why | Scope shifted during work |
| **§TRANSFER-BUDGET** | Cost/loss tradeoff at each fidelity | Always at handoff |

Full section specifications: [`spec/sections.md`](spec/sections.md)

## Transfer Budget

The key v0.5 innovation. At handoff time, AIST shows you exactly what each compression level costs and what it loses:

```
§TRANSFER-BUDGET
total-captured: ~950 tokens

| Fidelity | Tokens | What's lost                                    |
|----------|--------|------------------------------------------------|
| MAX      | ~950   | Nothing                                        |
| HIGH     | ~700   | CALIBRATION pairs, WORKING-STYLE               |
| MEDIUM   | ~450   | + HEURISTICS, SIGNIFICANCE                     |
| LOW      | ~250   | + IDEAS, ARTIFACTS, IMPLEMENTATION              |
| MIN      | ~120   | Everything except ESSENCE + MEMORY + HANDOFF   |

recommendation: HIGH — calibration pairs took 40 minutes of real
conversation. Worth preserving unless context is extremely tight.
```

You decide. The protocol never silently drops information.

## Templates

Ready-to-use templates for different scenarios:

| Template | Fidelity | Sections | Use case |
|----------|----------|----------|----------|
| [`handoff-max.aist`](templates/handoff-max.aist) | MAX | All | Default — use when context allows |
| [`handoff-full.aist`](templates/handoff-full.aist) | HIGH | Core + decisions + threads | Cross-tool transfers |
| [`handoff-standard.aist`](templates/handoff-standard.aist) | LOW | Essence + memory + handoff | Tight context budgets |
| [`handoff-minimal.aist`](templates/handoff-minimal.aist) | MIN | Bare essentials | Emergency resume |
| [`handoff-merge.aist`](templates/handoff-merge.aist) | MAX | All + merge sections | Combining parallel sessions |

## Tools

### Context Cost Calculator

Interactive calculator showing what AIST costs across 68 models in 22 languages.

**[Try it live](https://robiw-dev.github.io/aist-protocol/)** | [`tools/context-calculator.html`](tools/context-calculator.html)

### Visual Explainers

Infographics and lifecycle posters explaining AIST — use them in articles, talks, or social posts.

See [`visual/`](visual/) for the full set.

## Examples

| Example | Shows |
|---------|-------|
| [`self-handoff.aist`](examples/self-handoff.aist) | AIST preserving its own design sessions |
| [`creative-session.aist`](examples/creative-session.aist) | v0.5 features: §CALIBRATION, §HEURISTICS, §WORKING-STYLE |
| [`parallel-merge.aist`](examples/parallel-merge.aist) | Merging two concurrent work streams |

## Design Principles

**Capture everything, compress at transfer.** During work, every section gets populated. At handoff time, the Transfer Budget shows cost/loss tradeoffs. The user decides what to keep. Nothing is silently dropped.

**Model-agnostic.** AIST is plain text with lightweight structure. Any LLM that reads text can consume and generate AIST handoffs. No vendor lock-in, no proprietary format.

**Constant size.** Unlike copy-paste context that grows with every session, an AIST handoff stays the same size forever. Session 50 costs the same tokens as session 2.

**Human-readable.** You can read, edit, and version-control AIST handoffs. They're Markdown-adjacent text files, not binary blobs.

**Backward-compatible.** v0.5 is additive to v0.4. Older handoffs parse fine. New sections are optional — technical projects skip §CALIBRATION naturally.

## Specification

The full v0.5 spec lives in [`spec/`](spec/):

- [`aist-v05.md`](spec/aist-v05.md) — Protocol specification
- [`sections.md`](spec/sections.md) — Complete section definitions with examples

## License

- **Specification and templates:** [CC BY-SA 4.0](LICENSE) — use it, adapt it, share it. Derivatives must stay open.
- **Tools and code:** [Apache 2.0](LICENSE-TOOLING) — includes patent grant, enterprise-friendly.

## Contributing

AIST was built for one person's workflow. Fork it. Change everything. The approach matters more than the format.

If you find it useful, the best contribution is sharing how you adapted it — what sections you added, what you dropped, what problems you solved that this version doesn't address.

See [`CONTRIBUTING.md`](CONTRIBUTING.md) for details.

---

*Built by a developer who got tired of re-explaining the same project to the same AI every morning.*
