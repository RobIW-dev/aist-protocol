# AIST Section Specifications

Complete specification for all AIST v0.5 sections.

## §HEADER

**Required.** Metadata for routing and lineage.

```
§HEADER
project: project-name
session: 2026-01-30T12:00:00Z
lineage: parent:abc123 merges:def456,ghi789
context: claude.ai→claude-code
version: 7
```

| Field | Purpose | Format |
|-------|---------|--------|
| `project` | Project namespace | kebab-case |
| `session` | When captured | ISO 8601 |
| `lineage` | Related sessions | parent:id merges:id,id continues:id |
| `context` | Transfer type | source→destination |
| `version` | Project iteration | number |

## §STREAM (v0.4)

**New in v0.4.** Session identity and relationships for parallel work.

```
§STREAM
id: session-unique-id
type: continuation | fork | merge-result
continues: parent-session-id
forked-from: source-session-id
parallel-to: sibling-session-id
merges:
  - id: abc123
    name: stream-a-name
    focus: what-that-stream-did
  - id: def456
    name: stream-b-name
    focus: what-that-stream-did
```

| Field | Purpose |
|-------|---------|
| `id` | Unique session identifier |
| `type` | Session relationship type |
| `continues` | Linear continuation of previous |
| `forked-from` | Branched from this point |
| `parallel-to` | Concurrent sibling session |
| `merges` | Multiple streams being unified |

## §FOUNDATION (v0.4)

**New in v0.4.** Shared starting point for parallel streams or merges.

```
§FOUNDATION
codebase-state: Tauri 2.0 + React 18 + TipTap 2.x
ui-framework: FloatingPane, Sidebar complete
existing-panels: Ideas, Notes, Settings
existing-stores: appStore, settingsStore
known-constraints: Local-first, desktop-only
```

Use when merging parallel streams to establish what was common before divergence.

## §ESSENCE

**Required.** Project soul in one breath. Max 50 tokens.

```
§ESSENCE
Oracle lag exploitation in PM binary markets. 40-70% pressure = 90% win. 
Moat: historical depth.
```

## §MEMORY

**Required.** Key facts the receiving context needs.

```
§MEMORY
+user: sr-dev, 15y-exp, constraint-evaporation-mindset
+project.state: v7-running, validated
+discovery: script-tasks-are-plain-text
!blocker: need-track-record
!critical: tldraw-license-not-MIT
-deprecated: v5-approach
```

| Prefix | Meaning |
|--------|---------|
| `+` | Active fact |
| `!` | Warning/blocker (load into working memory immediately) |
| `-` | Removed/outdated |

## §DECISIONS (v0.4 Enhanced)

**Recommended.** What was decided, why, what was rejected, AND what we liked about rejections.

```
§DECISIONS
[D1] 2026-01-30 "Reject BimlExpress"
     why: vendor-lock-in-unacceptable
     rejected: BimlExpress(proprietary), raw-XML(painful)
     liked: BimlExpress had great intellisense, XML approach was simple
     revisit-when: BimlExpress goes open-source OR we need enterprise support
     
[D2] 2026-01-30 "ManagedDTS as backend"
     why: official-MS-API, handles-hard-stuff
     evaporated: "need-live-DB" → parse-local-files

[D3] 2026-02-01 "excalidraw-over-tldraw"
     why: MIT license allows full OSS including commercial
     rejected: tldraw (custom license, paid for commercial)
     liked: tldraw has cleaner SDK, better programmatic API, "Make Real" AI feature
     revisit-when: tldraw license changes OR building non-commercial fork
     importance: HIGH
```

| Field | Required | Purpose |
|-------|----------|---------|
| `why` | Yes | Rationale for decision |
| `rejected` | Yes | Alternatives considered + brief reason |
| `liked` | No | What was good about rejected options (preserves seeds) |
| `revisit-when` | No | Conditions that would reopen this decision |
| `supersedes` | No | Previous decision this replaces |
| `evaporated` | No | Constraint that dissolved |
| `importance` | No | HIGH/MEDIUM/LOW — affects whether to load §SIGNIFICANCE entry |

**Why `liked:` and `revisit-when:` matter:**
- Rejected alternatives often contain seeds of future value
- Without these fields, the nuance of "good option, wrong time" is lost
- Enables future sessions to reconsider when conditions change

## §EVOLUTION (v0.4)

**New in v0.4.** Tracks scope changes during work.

```
§EVOLUTION
type: subsumption | expansion | pivot | contraction
from: MVP (good enough for personal use)
to: PREMIUM (market-competitive)
trigger: Constraint evaporation — visual quality was effort-based, not technical
active-scope: PREMIUM
regression-guard: Foundation supports premium — do NOT propose MVP compromises
subsumes: MVP functionality preserved but vision expanded
```

| Field | Purpose |
|-------|---------|
| `type` | How scope changed |
| `from` | Previous scope/vision |
| `to` | New scope/vision |
| `trigger` | What caused the change (often constraint evaporation) |
| `active-scope` | Which scope is currently authoritative |
| `regression-guard` | Instruction to prevent ambition decay |
| `subsumes` | When B contains A+more, what A contributed |

### Evolution Types

| Type | Meaning |
|------|---------|
| `subsumption` | New scope contains old scope + more |
| `expansion` | Scope grew in specific dimension |
| `pivot` | Scope changed direction |
| `contraction` | Scope intentionally reduced |

## §THREADS

**Recommended.** Open work streams.

```
§THREADS
[T1] type-mapping status:designed effort:2h
     next: implement TypeRosettaStone class
     
[T2] poc status:ready effort:4h
     blocked: T1
     next: load existing package
     parallel: Can build alongside T3
```

| Status | Meaning |
|--------|---------|
| `not-started` | Hasn't begun |
| `in-progress` | Currently active |
| `blocked` | Waiting on something |
| `ready` | Ready to start |
| `complete` | Done |
| `deferred` | Intentionally postponed |

## §IMPLEMENTATION (v0.4)

**New in v0.4.** Bug fixes and solutions discovered during work.

```
§IMPLEMENTATION
[I1] title:"FloatingPane minimize race condition"
     problem: Multiple clicks needed for minimize/maximize/close
     solution: Added micro-interaction debounce, fixed state race
     files: FloatingPane.tsx
     
[I2] title:"Context bar modal positioning"
     problem: Modal disappeared off-screen depending on sidebar position
     solution: Smart popover positioning with viewport awareness
     files: ContextBar.tsx
```

| Field | Required | Purpose |
|-------|----------|---------|
| `title` | Yes | Brief description |
| `problem` | Yes | What was broken/wrong |
| `solution` | Yes | How it was fixed |
| `files` | Yes | Which files were changed |

**Why this section exists:** v0.3 stress tests showed 100% loss of implementation details. Without this, future sessions encounter the same bugs and reinvent the same solutions.

## §CONFIGURATION (v0.4)

**New in v0.4.** Settings inventory — what users can configure.

```
§CONFIGURATION
ui.sidebar-position: left | right | top | bottom
ui.context-bar-placement: sidebar | pane | window
ai.presence-level: off | manual | passive | active | engaged
editor.remain-open-on-save: boolean
```

Useful for:
- Knowing what's configurable vs hard-coded
- Understanding user customization surface
- Planning new settings without conflicts

## §PERFORMANCE (v0.4)

**New in v0.4.** Bundle sizes, lazy loading, performance-critical info.

```
§PERFORMANCE
bundle.main: ~1.2MB
bundle.mermaid: ~1.4MB (lazy loaded)
bundle.excalidraw: ~2MB (planned, must lazy load)
strategy: Code splitting via dynamic imports
warning: Excalidraw is largest — skeleton UI during load
```

## §IDEAS (v0.4)

**New in v0.4.** Captured ideas with archive synchronization tracking.

```
§IDEAS
[ID1] "Block templates library" synced:no
     Pre-built diagram starters (user journey, architecture)
     
[ID2] "AI block generation" synced:pending
     Type description → Claude generates Mermaid/Chart inline
     similar: idea-archive#142 (enrich existing)
     
[ID3] "Export to multiple AI targets" synced:yes archive-ref:idea-archive#347
     Why just Claude? DALL-E + Midjourney too
     constraint-evaporation: true
     
[ID4] "Cross-block linking" synced:no
     Click element in Excalidraw → jumps to related section
```

| Field | Required | Purpose |
|-------|----------|---------|
| `synced` | Yes | Sync status: `no` / `pending` / `yes` |
| `similar` | No | Existing archive entry to enrich instead of duplicate |
| `archive-ref` | No | Reference to synced archive entry |
| `constraint-evaporation` | No | Flag ideas that question assumed limits |

### Sync Status Values

| Status | Meaning | Action |
|--------|---------|--------|
| `no` | New idea, not yet reviewed | Offer to add to archive |
| `pending` | Similar exists, needs human decision | Show both, ask whether to merge |
| `yes` | Already in archive | No action needed |

### Archive-Sync Workflow

When generating AIST:
1. For each captured idea, check idea-archive for similar entries
2. If similar exists → set `synced:pending`, add `similar:` reference
3. If new → set `synced:no`
4. If already synced → set `synced:yes`, add `archive-ref:`

Before session ends:
1. Surface all `synced:no` and `synced:pending` ideas
2. Ask: "5 ideas captured. 2 match existing archive entries. Add/merge?"
3. Update sync status after human confirmation

**Why this matters:** Ideas captured in §IDEAS but never transferred to idea-archive are lost when the AIST is superseded. The sync tracking ensures nothing falls through the cracks.

## §ARTIFACTS

**References visual/structural content.**

```
§ARTIFACTS
[A1] type:architecture name:"Solution Overview"
     semantic: YAML-optional-to-ManagedDTS-to-DTSX-pipeline
     file: artifacts/architecture.md
     essential: true
     
[A2] type:reference name:"Type Rosetta Stone"
     semantic: Complete-DT-to-SQL-to-CSharp-mapping
     file: artifacts/type-mapping.md
     reusable: true
```

### Artifact Types

| Type | Purpose |
|------|---------|
| `architecture` | System structure, relationships |
| `reference` | Lookup tables, mappings |
| `evaporation` | Constraint analysis |
| `comparison` | Option evaluation, trade-offs |
| `flow` | Process/workflow diagrams |
| `code` | Code examples, patterns |

### Fields

| Field | Required | Purpose |
|-------|----------|---------|
| `type` | Yes | Category |
| `name` | Yes | Human-readable title |
| `semantic` | Yes | Meaning in one line (survives compression) |
| `file` | Yes | Path relative to HANDOFF.aist |
| `essential` | No | Load immediately (default: false) |
| `reusable` | No | Candidate for skill extraction (default: false) |

## §SIGNIFICANCE (v0.4)

**New in v0.4.** Breakthroughs, pivots, and dead-ends worth remembering.

```
§SIGNIFICANCE
[B1] type:breakthrough title:"Excalidraw license clarity"
     insight: tldraw requires commercial payment. Excalidraw is MIT.
     impact: HIGH — decides entire visual blocks strategy
     
[B2] type:pivot title:"MVP → Premium scope"
     insight: "Good enough" expanded to "competes with paid tools"
     trigger: Constraint evaporation revealed quality was effort-limited
     impact: HIGH — changes product positioning
     
[B3] type:dead-end title:"tldraw consideration"
     what: Considered tldraw for canvas layer
     why-abandoned: License incompatible with OSS commercial goals
     seed: Revisit if license changes or for non-commercial fork
```

| Type | When to Use |
|------|-------------|
| `breakthrough` | Insight that unlocked progress |
| `pivot` | Direction change |
| `dead-end` | Path abandoned (preserve the seed) |
| `discovery` | Unexpected finding |

## §CONSTRAINTS

**Recommended.** Hard limits vs evaporated constraints.

```
§CONSTRAINTS
HARD:
  - execution-requires-SSIS-engine (technical)
  - tldraw-license-not-MIT (legal)
  
EVAPORATED:
  - "VS-required-for-editing" → DTSX-is-XML
  - "notes-cant-match-figma" → effort-constraint-not-technical

QUESTIONING:
  - "agent-layer-needs-API" → webview-injection-may-work
```

## §ENERGY

**Recommended.** Approach, focus, momentum.

```
§ENERGY
mode: excited-validated
focus: implementation
momentum: high
approach: thorough-then-build
```

| Field | Values |
|-------|--------|
| `mode` | exploring, executing, debugging, frustrated, excited |
| `focus` | broad-ideation, narrow-implementation, polish |
| `momentum` | high, medium, low, stalled |
| `approach` | thorough, rapid-iteration, careful-validation |

## §HANDOFF

**Situational.** Instructions for next context.

```
§HANDOFF
to: claude-code
focus: implement tiered retention
skip: re-discussing architecture (settled)
warn: don't suggest keep-all (rejected D5)
first-task: T2
artifact-priority: A1, A2
regression-guard: scope-precedence:PREMIUM
context-files:
  - src/retention_manager.py
```

| Field | Purpose |
|-------|---------|
| `regression-guard` | Prevent scope/ambition decay |

## §CALIBRATION (v0.5)

**New in v0.5.** Preserves iterative creative refinement through before/after pairs with reasoning. The core section for creative state transfer.

```
§CALIBRATION
target: robiw-voice-dna

[C1] "staged-to-natural"
     before: "Tested GitHub Copilot on a production stored procedure..."
     after: "ChatGPT rewrote a query and made it significantly slower. Missed the obvious index."
     why: Staged evaluations read as content-creator farming engagement. Real practitioners notice things while working.

[C2] "remove-dramatic-closers"
     before: "...In production that string is half the codebase."
     after: "...The string just passes through untouched."
     why: If the observation is done, stop. No stump lines designed to raise eyebrows.

[C3] "non-native-phrasing"
     before: "Completely different skill."
     after: "Entirely different job."
     why: Shorter, more direct. How a Swedish engineer writes in English. Don't overcorrect into copywriting.
```

| Field | Required | Purpose |
|-------|----------|---------|
| `target` | Yes | What was being calibrated (skill name, document, voice) |
| `before` | Yes | What was wrong (short — just the key phrase) |
| `after` | Yes | What replaced it (short) |
| `why` | Yes | Transferable principle — must be reusable, not just "user said change it" |

**Why before/after pairs?**
A rule like "don't use dramatic closers" is ambiguous. A new Claude doesn't know where the boundary is. But seeing `"half the codebase" → "passes through untouched"` makes the judgment *feel-able*. The pair IS the knowledge.

**Rules:**
- `before` and `after` should be SHORT — the minimum to show the contrast
- `why` captures the transferable principle, not just "user didn't like it"
- 3-8 pairs is enough for most creative sessions
- If a skill already has the anti-pattern baked in, the AIST pair is redundant at MAX fidelity but ensures portability if the skill isn't loaded

## §HEURISTICS (v0.5)

**New in v0.5.** Judgment rules discovered through work, stated as if/then/test patterns. Extracted from §CALIBRATION pairs but abstracted into reusable self-checks.

```
§HEURISTICS
scope: robiw-voice

[H1] "no-staged-evaluations"
     if: Tweet describes sitting down to test/evaluate a tool
     then: Rewrite as something noticed while working
     test: "Would someone actually tweet this in the moment?"

[H2] "no-dramatic-stumps"
     if: Tweet ends with a short dramatic sentence designed to land
     then: Cut it. End where the observation naturally ends.
     test: "Remove the last sentence. Is the tweet worse? If not, it was a stump."

[H3] "no-sweeping-claims"
     if: Tweet uses "every tool" / "all X" / "no Y does Z"
     then: Name the specific tool and what happened
     test: "Could someone reply with a counterexample and make me look stupid?"

[H4] "non-native-natural"
     if: Phrasing sounds too literary or uses complex English constructions
     then: Shorter sentence. More direct.
     test: "Would I actually type this in a Slack message?"
```

| Field | Required | Purpose |
|-------|----------|---------|
| `scope` | Yes | What domain these apply to |
| `if` | Yes | Trigger condition |
| `then` | Yes | Action/replacement |
| `test` | Yes | Self-check question — one sentence the AI asks itself while generating |
| `confidence` | No | `single-session` / `multi-session` / `validated` |

**Relationship to §CALIBRATION:**
- §CALIBRATION = the evidence (specific examples)
- §HEURISTICS = the extracted rules (reusable patterns)
- Both are needed. Rules without examples are ambiguous. Examples without rules don't generalize.

**Rules:**
- `test` is the key field — it's what makes heuristics actionable
- Heuristics should be falsifiable — "make it better" is not a heuristic
- 4-8 per creative domain is the sweet spot
- Optional `confidence` field tracks whether rule is from one session or validated across many

## §WORKING-STYLE (v0.5)

**New in v0.5.** How the human collaborates. Not personality — workflow mechanics that affect response quality.

```
§WORKING-STYLE
feedback: concise, points to specific words/phrases, gives their real version
iteration: fast — fix and show, don't ask permission
framing: "this is not what I'd write" > "this needs work" — provides the actual alternative
tolerance: low for filler/drama, high for imperfection
language: English 2nd/3rd — influences what "natural" means for output
```

| Field | Required | Purpose |
|-------|----------|---------|
| Each key | No | Freeform key-value pairs |
| `scope` | No | Add `scope: project-name` for per-project override. No scope = global. |

**Rules:**
- Max 5-6 lines — focus on mechanics that actually change how the AI should behave
- Not personality traits — workflow traits
- "feedback: concise" means don't ask 3 clarifying questions, just fix it
- Global by default. Add `scope:` for per-project overrides.

## §TRANSFER-BUDGET (v0.5)

**New in v0.5. Required at handoff.** Shows the explicit cost/loss tradeoff for each fidelity level.

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

| Field | Required | Purpose |
|-------|----------|---------|
| `total-captured` | Yes | Total tokens if everything is included |
| `context-cost-at-max` | Yes | Percentage of next session's context window |
| Fidelity table | Yes | All levels with concrete loss descriptions |
| `recommendation` | Yes | Which level and WHY, in concrete terms |

**Rules for generating the budget:**
- Always show the full fidelity table
- Always show a recommendation with reasoning
- Frame losses concretely: "3 rounds of voice calibration" not "§CALIBRATION section"
- Show context cost as percentage, not just tokens
- If MAX is <5% of context, recommend MAX by default
- If MAX is >10% of context, flag it and suggest what to compact first
- User can respond: "MAX", "keep it tight", "CUSTOM: keep X drop Y", or just "go"

## Fidelity Levels (replaces v0.4 Profiles)

v0.4 had rigid profiles (MINIMAL, STANDARD, FULL, MERGE) that decided what to include. v0.5 replaces these with fidelity levels that decide how much to compress:

| Level | Approach | Use when |
|-------|----------|----------|
| MAX | Everything, no compression | Default. Context budget allows it. |
| HIGH | Drop low-value sections, compact others | Large sessions, moderate budget |
| MEDIUM | References to skills instead of inline content | Skills are installed, pairs are redundant |
| LOW | Core state only + threads + handoff | Quick resume, same person |
| MIN | Essence + memory + handoff | Just need to remember what we were doing |
| CUSTOM | User specifies what to keep/drop | User knows what matters |

**Key difference from v0.4:** The protocol always captures at MAX internally. Fidelity only affects what's *transferred*. Nothing is lost until the user explicitly chooses to compress it.
