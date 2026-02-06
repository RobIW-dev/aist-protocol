# AIST v0.5 Specification

> AI State Transfer Protocol — Lossless-first, evolution-aware knowledge transfer between AI contexts.

**Version:** 0.5  
**License:** CC BY-SA 4.0  
**Status:** Stable

## Core Philosophy

**Capture everything. Compress at transfer time. Never silently drop information.**

v0.4 used profiles that excluded sections — MINIMAL skipped §IDEAS, STANDARD skipped §CALIBRATION. Information was lost by default and the user never knew what was missing.

v0.5 flips this: every section is populated during work. At handoff time, the protocol shows a **Transfer Budget** — what each fidelity level costs in context, what it preserves, and what it loses. The user decides.

## What's New in v0.5

| v0.4 Gap | v0.5 Fix |
|----------|----------|
| Creative calibration lost | §CALIBRATION preserves before/after pairs with reasoning |
| Judgment rules lost | §HEURISTICS captures discovered if/then/test patterns |
| Collaboration style implicit | §WORKING-STYLE captures workflow mechanics |
| Profiles silently drop sections | Transfer Budget shows cost/loss explicitly |
| Fixed compression levels | Fidelity is a dial, not a switch |

## Quick Reference

```
@AIST/5.0

§HEADER
project: name | session: ISO-timestamp | lineage: merges:id1,id2

§STREAM
id: session-id | type: merge-result
merges: [{id, name, focus}, ...]

§FOUNDATION
codebase-state: shared-start | known-constraints: list

§ESSENCE
One-breath summary (~50 tokens)

§MEMORY
+key: fact | !key: warning | -key: deprecated

§DECISIONS
[D1] date "slug"
     why: rationale | rejected: option(reason)
     liked: what-was-good | revisit-when: conditions

§EVOLUTION
type: subsumption | from: old-scope | to: new-scope
trigger: what-caused-change | regression-guard: instruction

§THREADS
[T1] name status:ready effort:2h | next: action

§IMPLEMENTATION
[I1] title:"description" | problem: what | solution: how | files: list

§CALIBRATION                                          # NEW v0.5
target: what-was-calibrated
[C1] "slug" | before: old | after: new | why: principle

§HEURISTICS                                           # NEW v0.5
scope: domain
[H1] "slug" | if: condition | then: action | test: self-check question

§WORKING-STYLE                                        # NEW v0.5
feedback: how-user-gives-feedback
iteration: how-fast-to-cycle
tolerance: what-they-accept-vs-reject

§IDEAS
[ID1] "name" synced:no | description

§ARTIFACTS
[A1] type:arch name:"title" | semantic: meaning | file: path

§SIGNIFICANCE
[B1] type:breakthrough | insight: what | impact: HIGH

§HANDOFF
to: target | focus: task | regression-guard: scope-precedence:PREMIUM

§TRANSFER-BUDGET                                      # NEW v0.5
(auto-generated at handoff time — see Transfer Budget below)
```

## Sections

| Section | Purpose | When |
|---------|---------|------|
| §HEADER | Metadata | Always |
| §STREAM | Parallel/merge identity | Parallel work |
| §FOUNDATION | Shared start | Merges |
| §ESSENCE | Project soul | Always |
| §MEMORY | Key facts | Always |
| §DECISIONS | Choices + rejected + liked | Decisions made |
| §EVOLUTION | Scope changes | Scope evolved |
| §THREADS | Work streams | Tasks exist |
| §IMPLEMENTATION | Bug fixes | Fixes discovered |
| §CALIBRATION | Before/after creative refinement | Creative/voice/design work |
| §HEURISTICS | Discovered judgment rules | Rules emerged from work |
| §WORKING-STYLE | Collaboration mechanics | Always (global or per-project) |
| §IDEAS | Ideas + sync status | Ideas emerged |
| §ARTIFACTS | Visual/structural refs | Diagrams/files exist |
| §SIGNIFICANCE | Breakthroughs | Major insights |
| §HANDOFF | Next instructions | Handing off |
| §TRANSFER-BUDGET | Cost/loss display | Always at handoff |

For complete specs: `references/sections.md`
For examples: `references/examples.md`

## Transfer Budget

The key v0.5 innovation. At handoff time, generate a budget that shows:

```
§TRANSFER-BUDGET
total-captured: ~800 tokens (18 sections populated)
context-cost-at-max: ~0.5% of 200K window | ~12% of 8K window

| Fidelity | Tokens | What's lost                                            |
|----------|--------|--------------------------------------------------------|
| MAX      | ~800   | Nothing                                                |
| HIGH     | ~600   | ENERGY, PERFORMANCE, compact MEMORY                    |
| MEDIUM   | ~400   | + CALIBRATION pairs (refs to skill only)                |
|          |        | + HEURISTICS (refs to skill only)                      |
| LOW      | ~200   | + IMPLEMENTATION, IDEAS, ARTIFACTS, WORKING-STYLE      |
| MIN      | ~100   | Everything except ESSENCE + MEMORY + HANDOFF.          |
|          |        | Enough to resume, not to rebuild.                      |
| CUSTOM   | varies | "Keep CALIBRATION, drop IMPLEMENTATION"                |

recommendation: MAX — under 1% on most models, worth preserving 3 rounds of
voice calibration that took 40 minutes of real conversation.
```

**Rules for Transfer Budget:**
- Always show the full table
- Always show a recommendation with reasoning
- Frame losses concretely: "3 rounds of voice calibration" not "§CALIBRATION section"
- Show context cost as percentage, not just tokens
- If MAX is <5% of next session context, recommend MAX by default
- User can say "CUSTOM: keep X, drop Y" or just "MAX" or "keep it tight"

## Key Concepts

**Lossless-first:** Capture everything during work. Compress only at transfer.

**Transfer Budget:** Explicit cost/loss tradeoff shown to user. Never silently drop.

**Parallel-first:** Assumes concurrent streams, not linear A-B-C.

**Subsumption:** B contains A + more. Preserve A's work, adopt B's vision.

**Regression guards:** Prevent scope decay in future sessions.

**Archive-sync:** Ideas track `synced:no|pending|yes` status.

**Calibration pairs:** Before/after examples ARE the knowledge. Rules without examples are ambiguous.

**Heuristic tests:** Self-check questions the AI applies while generating.

## Backward Compatibility

- v0.5 is additive — all v0.4 sections unchanged
- v0.4 handoffs parse fine in v0.5 — missing sections mean no creative context
- Version tag: `@AIST/5.0` when new sections used, `@AIST/4.0` still valid
- New sections are always optional — technical-only projects skip §CALIBRATION naturally
