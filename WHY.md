# Why AIST Exists

## The Short Version

AI sessions are disposable by default. Every conversation starts from zero. Your AI doesn't remember the decisions you made, the approaches you rejected, or the calibration that took real effort.

AIST makes sessions cumulative instead of disposable.

## The Origin

I'm a database developer. 15 years of production T-SQL, SSIS, SQL Server. When I started working with AI seriously, I noticed the same pattern every morning: re-explain the project, re-state the constraints, re-derive the decisions I'd already made. Each session felt productive in isolation, but the work wasn't compounding.

The breakthrough wasn't a technical insight. It was noticing that most of what I was re-explaining could be compressed into a structured document — decisions with rationale, rejected alternatives, key facts, next steps — and that this document could be tiny compared to the conversation that produced it.

A 40,000-token conversation about architecture decisions compresses to ~950 tokens of structured state. 60x compression. And that 950-token handoff carries everything the next session needs to resume at full speed.

I built AIST for my own workflow. Then I realized no public equivalent existed.

## The Anti-Incentive Problem

Why hasn't this been built before? Because the organizations best positioned to build it have no incentive to do so.

AI providers monetize tokens. Every token you spend re-explaining context is revenue. Every session that starts from zero means more API calls, more subscription usage, more compute. A protocol that reduces context overhead by 60x works against the business model.

This isn't a conspiracy — it's a structural incentive. The solution has to come from outside the providers, built by people who use these tools and want them to work better.

That's why AIST is open-source under CC BY-SA 4.0. The share-alike clause ensures derivatives stay open. Nobody wraps this and sells it back to you.

## Three Things That Matter

### 1. Sessions Should Be Cumulative

The default assumption in AI tooling is that sessions are independent. Start fresh, work, close, repeat. Platform memory captures personality preferences ("user prefers concise answers") but not project state ("we rejected session tokens because they don't scale horizontally, chose OAuth2 + PKCE instead, and the rate limiting design is blocked on Redis connection pooling").

These are different categories of knowledge:

| Type | Example | Platform memory captures? |
|------|---------|--------------------------|
| Preference | "I prefer Python over Java" | Yes |
| Decision | "We chose PostgreSQL over DynamoDB because..." | No |
| Rejection | "We tried event sourcing and abandoned it because..." | No |
| Calibration | "The voice needs to sound less formal, more practitioner" | No |
| Warning | "Don't touch the billing module — it has undocumented side effects" | No |
| Architecture | "The plugin registry uses a discovery pattern, not explicit registration" | No |

AIST captures the second category — the project-specific state that makes sessions productive. Platform memory and AIST are complementary, not competing.

### 2. The Least Privileged Benefit the Most

An AIST handoff is roughly 950 tokens. On a 200K context window, that's 0.5% — barely noticeable. On a 1M context window, it's 0.1%.

But on an 8K free tier, it's 12%. On a 4K local model, it's 24%. That's significant — and it's also the only way these users get multi-session continuity at all.

Without AIST, context recaps grow with every session. You paste more and more history until it fills the window. On large models, this is wasteful but survivable. On small models, it's fatal — the recap alone consumes the context window after 2-3 sessions, leaving no room for actual work.

AIST's constant-size handoff changes this equation. Session 2 and session 50 cost the same 950 tokens. The project doesn't outgrow the model.

This means:

- **Free tier users** (8K context) can run multi-week projects
- **Local model users** (4K-32K) get genuine session continuity
- **API users on a budget** spend tokens on work, not recap
- **Developers in countries where $20/month matters** get access to sustained AI collaboration

The value of AIST is inversely proportional to context size. The people with the least resources benefit the most.

### 3. Small Models Can Do Sustained Work

The minimum viable context for multi-session project work without AIST is roughly 32K tokens — enough to hold a meaningful recap plus actual work. Below that, the recap alone crowds out the working space.

With AIST, that minimum drops to roughly 4K. A structured 950-token handoff leaves room for work even on the smallest models.

A $200 used GPU running a 7B model can now maintain project continuity across sessions. A Raspberry Pi running a quantized model can sustain a multi-week coding project. These aren't theoretical — they're configurations that are genuinely infeasible without structured state transfer and genuinely functional with it.

This isn't optimization for optimization's sake. It's expanding who can do serious AI-assisted work.

## What AIST Isn't

**AIST is not a product.** It's a specification. There's no service to sign up for, no API key, no subscription. You paste text into your AI and it works.

**AIST is not RAG.** RAG retrieves fragments from a corpus. AIST preserves the structured state of a specific project — decisions, rejections, warnings, next steps, calibration. These are different tools for different problems.

**AIST is not memory.** Platform memory captures who you are. AIST captures where a project is. You need both.

**AIST is not a compression algorithm.** It's a protocol for deciding what to keep and what to lose, with explicit tradeoffs shown to the user. The "compression" is structured summarization, not lossless encoding.

## The Recursive Proof

AIST v0.5 was designed across multiple AI sessions. The protocol that preserves session state was used to preserve the state of its own design process. The self-handoff in [`examples/self-handoff.aist`](examples/self-handoff.aist) is both a functional example and proof that the format works.

The design sessions produced ~80,000 tokens of conversation. The self-handoff is 650 tokens. That's 120x compression with every design decision, rejected alternative, and architectural rationale preserved.

## What Happens Next

AIST is published as-is. No roadmap, no feature promises, no community governance structure. It's one developer's solution to a problem that turned out to be universal.

If you find it useful:

- **Use it.** Paste a handoff into your next AI session and see if it helps.
- **Adapt it.** Add sections, remove sections, change the format. The approach matters more than the spec.
- **Share what you learn.** The most valuable feedback is how you changed AIST for your workflow — what the spec doesn't cover that it should.

If nobody uses it, that's fine too. It already solved my problem.

---

*"Your AI forgets everything between sessions. Here's the 950-token protocol that fixes it."*
