# Agentic Development Framework

A 16-step structured development process for AI coding agents. Designed to reduce rework, prevent repeated mistakes, and ensure what gets built matches what was asked for.

## The Problem

AI coding agents are powerful but fail in predictable ways:
- They lose context across sessions and repeat the same mistakes
- They start coding before confirming what the user actually wants
- They verify code works but not that it meets requirements
- They don't learn from debugging dead ends
- They introduce inconsistent patterns because they don't check existing code
- They deploy stale builds and debug phantom bugs
- They expand scope to fill available compute
- They don't think about what could go wrong before building
- They treat security as an afterthought

Every step in this framework exists because its absence caused a specific, documented failure.

## Built From

Validated against 22+ engineering frameworks:

| Category | Frameworks |
|----------|-----------|
| AI Agent Workflows | BMAD, GitHub Spec Kit, Amazon Kiro, OpenSpec, RIPER-5, Memory Bank, spec-then-code, Aider |
| Professional Engineering | Google Engineering Practices, Microsoft SDL, Amazon ORR/COE, Meta DRS, DORA |
| Risk Management | Risk Up Front (RUF), Risk Storming, ATAM, Risk-First, Pre-Mortem Analysis |
| Development Methods | XP (TDD), Shape Up, Trunk-Based Development, Toyota/Lean |
| Reliability | Google SRE, Netflix Chaos Engineering, Blameless Postmortems |
| Documentation | ADRs, RFC Culture (Uber, HashiCorp, Google Design Docs) |

## Quick Start

### 1. Copy into your project

```bash
git clone https://github.com/rccypher/agentic-dev-framework.git
cp agentic-dev-framework/FRAMEWORK.md your-project/Docs/DEVELOPMENT_PROCESS.md
cp agentic-dev-framework/DISCOVERY.md your-project/Docs/DISCOVERY.md
cp -r agentic-dev-framework/templates your-project/Docs/templates
mkdir -p your-project/Docs/{archive,design-discussions,specs}
```

### 2. Add to your CLAUDE.md

See `install/CLAUDE_SNIPPET.md` for the snippet.

### 3. Use it

The framework routes tasks through 3 complexity tiers:

| Tier | When | Steps |
|------|------|-------|
| **Quick Fix** | Single file, obvious fix | 5 steps |
| **Standard** | 2-3 files, clear scope | 11 steps |
| **Complex** | 3+ modules, architectural | All 16 steps |

## The 16 Steps

```
 1. COMPLEXITY ASSESSMENT      Route + time budget + task type
 2. DESIGN DISCUSSION LOG      Discovery process + structured elicitation
 3. REQUIREMENTS CONFIRMATION  Restate, acceptance criteria, user confirms
 4. SPEC ARTIFACT              Formal spec with non-goals + pre-mortem + security
 5. EXPLORE                    Read code, identify modules, note patterns
 6. READ ARCHIVE               Invariants, dead ends, constraints, postmortems
 7. IMPACT ANALYSIS            Blast radius + risk storming + riskiest assumption
 8. PLAN                       Approach + alternatives + RAT + pre-mortem + tests + rollback
 9. WRITE TESTS                Failing tests before implementation (red phase)
10. IMPLEMENT                  Pattern conformance + scope completeness + Andon stops
11. VERIFY                     Build, deploy, tests green, ORR, self-review checklist
12. DEBUG                      Root cause + five whys + record dead ends in real time
13. REQUIREMENTS VERIFICATION  Walk spec with evidence, not assertions
14. FRESH-AGENT REVIEW         New session reviews spec + diff (zero conversation history)
15. ARCHIVE CAPTURE            Decisions, dead ends, prompt rationale, process reflection
16. DONE                       Checklist verification
```

## Key Files

| File | Purpose |
|------|---------|
| [FRAMEWORK.md](FRAMEWORK.md) | Full 16-step process with all details |
| [DISCOVERY.md](DISCOVERY.md) | Non-technical user elicitation guide with metaphors |
| [templates/spec.md](templates/spec.md) | Spec template with non-goals, pre-mortem, STRIDE |
| [templates/design-discussion.md](templates/design-discussion.md) | Design discussion with discovery and DKDK |
| [templates/archive-entry.md](templates/archive-entry.md) | Archive entry format and guidelines |
| [templates/coe-postmortem.md](templates/coe-postmortem.md) | Incident analysis with Five Whys and feedback loop |
| [install/CLAUDE_SNIPPET.md](install/CLAUDE_SNIPPET.md) | CLAUDE.md integration snippet |

## What Makes This Different

**Unique to this framework (not found in any surveyed methodology):**
- Dead-end recording during active debugging
- Prompt/LLM rationale capture in the archive
- Scope boundary confirmation as an explicit gate
- Pre-plan archive review for known failures
- Blast radius + caller/callee tracing before code changes
- Rollback path required in every plan
- Non-technical discovery process with practical metaphors
- Andon cord (hard stop conditions) during implementation
- COE feedback loop from incidents back into the framework itself

**Adopted and adapted from professional engineering:**
- Pre-mortem analysis (Gary Klein) → in spec and plan
- STRIDE threat modeling (Microsoft SDL) → in spec
- Operational Readiness Review (Amazon ORR) → in verify
- Five Whys root cause analysis (Amazon COE) → in debug and postmortems
- Self-review checklist (Google Engineering) → in verify
- Circuit breaker and time budgets (Shape Up) → in complexity assessment
- Test-first gates (XP TDD) → write tests step
- Fresh-agent review (OpenSpec) → catch implementation rationalizations
- Error budget tracking (Google SRE) → ongoing quality metric
- DKDK meetings for unknown unknowns (Risk Up Front) → in design discussion
- Riskiest Assumption Test (Lean) → in impact analysis and plan
- Risk Storming (Simon Brown) → in impact analysis
- Feature flags (Trunk-Based Development) → in implement

**v3.0 additions — Agent Team Structure & Prompting (empirical):**
- Single-agent default over multi-agent (Cursor, Windsurf, Copilot convergence)
- Artifact-driven communication, not conversational (MetaGPT 3.9 vs ChatDev 2.1)
- Model tiering: reasoning models for design/debug, standard for implementation
- Fresh context per implementation unit (BMAD, validated in production)
- No persona framing in prompts — domain checklists + methodology instead (arXiv:2603.18507: expert personas degrade coding accuracy by -0.65 on MT-Bench)
- Adversarial review framing with mandatory findings (BMAD TEA agent pattern)
- Tool permissions enforce role boundaries, not prompt instructions

## Task-Type Variants

| Type | Key Differences |
|------|----------------|
| Feature | Full pipeline (default) |
| Bug Fix | Current vs expected vs unchanged behavior |
| Refactoring | Tests pass before AND after; no new features |
| Security Fix | STRIDE mandatory; fresh-agent review mandatory; ship immediately |
| Prompt Engineering | A/B comparison on known inputs; regression testing |
| Infrastructure | ORR mandatory; rollback verification; canary deployment |

## Contributing

This framework evolves through use. Process reflections (step 15) identify friction. Open an issue or PR with improvements.

## License

MIT
