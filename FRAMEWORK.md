# Agentic Development Framework

**Version:** 3.0.0 (2026-04-17)

A structured development process for AI coding agents. Built from real-world failures and validated against 22+ professional engineering frameworks including BMAD, GitHub Spec Kit, Amazon Kiro, OpenSpec, RIPER-5, Memory Bank, ADRs, spec-then-code, Aider, Google Engineering Practices, Microsoft SDL, Amazon ORR/COE, Meta DRS, Toyota Production System, DORA, Chaos Engineering, Google SRE, XP, Shape Up, RFC culture, Pre-Mortem analysis, Risk Up Front (RUF), Risk Storming, ATAM, and Risk-First Software Development.

---

## Complexity Routing

Assess complexity first. Follow the appropriate tier.

| Tier | Criteria | Steps |
|------|----------|-------|
| **Quick Fix** | Single function/file, no schema change, obvious root cause, no new patterns | 5 → 10 → 11 → 15 (brief) → 16 |
| **Standard** | 2-3 files, clear scope, no architectural changes | 3 → 4 → 5 → 6 → 8 → 9 → 10 → 11 → 13 → 15 → 16 |
| **Complex** | 3+ modules, schema/API changes, new patterns, prompt engineering, architectural decisions | All 16 steps |

**When in doubt, go one tier up.** If during any step you realize the change is more complex than assessed, escalate and backfill skipped steps.

### Time Budget & Circuit Breaker (Shape Up)

Every task gets a time budget at assessment. If the budget is exhausted without completion:
- **Stop.** Do not continue.
- Review what's done and what's still unknown.
- Reduce scope or re-shape the task.
- Get user confirmation before proceeding.

**Midpoint checkpoint:** At 50% of the time budget, report what is still uncertain vs. what is planned. Uncertainty at midpoint triggers user review.

### Task-Type Variants

Different work types emphasize different steps:

| Task Type | Key Differences |
|-----------|----------------|
| **Feature** | Full creative pipeline (default) |
| **Bug Fix** | Document current vs. expected vs. unchanged behavior. Root cause before fix. |
| **Refactoring** | Tests must pass BEFORE and AFTER. Same behavior, different structure. No new features. |
| **Security Fix** | STRIDE threat model mandatory. Fresh-agent review mandatory. No feature flags — ship immediately. |
| **Prompt Engineering** | A/B comparison of outputs on known inputs. Regression test on existing behavior. |
| **Infrastructure** | ORR checklist mandatory. Rollback verification. Canary deployment where possible. |

---

## Process Overview

```
TASK RECEIVED
     |
     v
1. COMPLEXITY ASSESSMENT -----> Route + time budget + task type
     |
     v
2. DESIGN DISCUSSION LOG -----> Discovery process + structured elicitation
     |
     v
3. REQUIREMENTS CONFIRMATION -----> User confirms; Definition Meeting format
     |
     v
4. SPEC ARTIFACT -----> Formal spec with non-goals, pre-mortem, security
     |
     v
5. EXPLORE (read code, identify modules, note patterns)
     |
     v
6. READ ARCHIVE (invariants, dead ends, constraints, postmortems)
     |
     v
7. IMPACT ANALYSIS -----> Blast radius + risk storming; confirm if > 3 modules
     |
     v
8. PLAN (approach + alternatives + RAT + pre-mortem + test strategy + rollback)
     |                                          |
     v                                User confirms if non-trivial
9. WRITE TESTS (failing tests against acceptance criteria)
     |
     v
10. IMPLEMENT (pattern conformance + scope completeness + Andon stops)
     |
     v
11. VERIFY (build, deploy, tests green, ORR checklist, data validation)
     |
     v
  PASS? --NO--> 12. DEBUG (root cause, record dead ends) --> return to 10
     |
    YES
     |
     v
13. REQUIREMENTS VERIFICATION (walk spec criteria with evidence + self-review)
     |
     v
  ALL MET? --NO--> return to 10
     |
    YES
     |
     v
14. FRESH-AGENT REVIEW (new session: spec + diff only, no conversation history)
     |
     v
15. ARCHIVE CAPTURE (decisions, dead ends, prompt rationale, process reflection)
     |
     v
16. DONE
     |
  FAILURE IN PRODUCTION? --> TRIGGERED: COE / POSTMORTEM PROCESS
```

---

## Step Details

### 1. COMPLEXITY ASSESSMENT

**Goal:** Route the task to the appropriate tier, set time budget, identify task type.

**Applies to:** All tiers (this step determines the tier).

Actions:
- Assess dimensions: files affected, schema/API changes, new patterns, architectural impact, risk if wrong
- Identify task type: feature, bug fix, refactoring, security fix, prompt engineering, infrastructure
- Set time budget: "This task gets [X] hours/minutes of effort before a checkpoint"
- State explicitly: "This is a [tier] [task type] because [reason]. Time budget: [X]."

**Escalation rule:** If during ANY subsequent step you discover the change is more complex than assessed, stop and escalate. Backfill skipped steps.

---

### 2. DESIGN DISCUSSION LOG

**Goal:** Surface hidden requirements through structured discovery before formalizing anything.

**Applies to:** Complex (mandatory). Standard (optional). Quick fix (skip).

Actions:
- Create `docs/design-discussions/YYYY-MM-DD-<slug>.md`
- **Run the Discovery Process** (see `DISCOVERY.md`) — structured, non-technical elicitation that surfaces edge cases, constraints, and expectations
- **DKDK Probe** (Risk Up Front): Ask specifically about unknowns — "What about this task makes you uncertain? What might we be missing? What has gone wrong with similar work before?"
- Record the conversation as it happens, including the user's original words
- Note what was considered AND what was rejected, with reasons

---

### 3. REQUIREMENTS CONFIRMATION

**Goal:** Eliminate ambiguity. Achieve mutual understanding of every word.

**Applies to:** Standard and Complex. Quick fix (skip).

Actions:
- Restate the task in your own words
- List acceptance criteria: "This is done when X, Y, Z"
- List scope boundaries: "This does NOT include A, B"
- **Definition Meeting format** (RUF): Read each requirement back to the user statement by statement. Confirm understanding of every term. Surface disagreements about what words mean while the cost of correction is near zero.
- Identify unknowns or assumptions that need confirmation

**Gate:** Do not proceed until user confirms.

---

### 4. SPEC ARTIFACT

**Goal:** Produce a formal, versioned spec as the single source of truth.

**Applies to:** Standard and Complex. Quick fix (skip).

Actions:
- Generate `docs/specs/YYYY-MM-DD-<slug>.spec.md`
- **Must include Non-Goals** (Google Design Docs): explicit statements of what this implementation will NOT do. As important as requirements. Agents self-check against non-goals before declaring completion.
- **Must include Pre-Mortem section** (Gary Klein): "Assume this implementation has failed. List every reason it could have failed." Each failure mode becomes either a test case or a documented assumption.
- **Security section** (Microsoft SDL): For code touching auth, data storage, external APIs, or user input, include a STRIDE threat model (Spoofing, Tampering, Repudiation, Information Disclosure, DoS, Elevation of Privilege).
- The spec is a living document — update when requirements change, not conversation history

See `templates/spec.md` for the full template.

---

### 5. EXPLORE

**Goal:** Understand what exists before changing anything.

**Applies to:** All tiers.

Actions:
- Read relevant source files — do not propose changes to code you haven't read
- Understand current behavior
- Identify all affected modules/files
- Note existing patterns: how does similar code handle the same problem?
- **For bug fixes:** Document current behavior, expected behavior, and behavior that must remain unchanged

---

### 6. READ ARCHIVE

**Goal:** Learn from previous decisions and failures before repeating them.

**Applies to:** Standard and Complex. Quick fix (check if module has known gotchas).

Actions:
- Open archive file(s) in `docs/archive/` for each affected module
- Check invariants, dead ends, constraints
- **Check postmortem repository** — has this type of failure happened before? What was the root cause? What systemic change was made?
- If no archive exists for the module, create one in step 15

---

### 7. IMPACT ANALYSIS

**Goal:** Understand blast radius. Prevent collateral damage.

**Applies to:** Standard and Complex. Quick fix (skip).

Actions:
- List every file to be modified
- List callers and callees of modified code
- For DB changes: check schema constraints (UNIQUE, NOT NULL, foreign keys, triggers)
- Grep for shared patterns — same fix may be needed elsewhere
- **Risk Storming** (Simon Brown, for Complex tier): On architecture-level changes, identify risks in three domains: People (skill gaps), Process (workflow gaps), Technology (architectural inadequacy). Note where only one person has identified a risk — these are blind spots.
- **Riskiest Assumption Test** (RAT): What is the single assumption most likely to make this change fail? Can it be tested cheaply before full implementation?

**Gate:** If blast radius > 3 modules, confirm with user.

---

### 8. PLAN

**Goal:** Design the approach with full awareness of alternatives, risks, and failure modes.

**Applies to:** Standard and Complex. Quick fix (skip).

Actions:
- Design the implementation approach
- Verify plan does NOT violate archived invariants

- **ALTERNATIVES CONSIDERED** (mandatory for Complex, recommended for Standard):
  - Document at least one rejected approach with specific reason for rejection
  - Record in spec artifact under "Alternatives Considered"
  - Future agents need this to avoid rediscovering the same dead end as a "new idea"

- **PRE-MORTEM** (mandatory for Complex):
  - "This implementation has already failed. Why?"
  - Enumerate failure modes
  - Each becomes a test case (step 9) or documented assumption

- **RISKIEST ASSUMPTION TEST** (RAT):
  - Identify the assumption most likely to cause failure
  - Design a minimal probe to test it before full implementation
  - If the assumption fails, reshape the approach before investing further

- **SECURITY CHECKPOINT** (mandatory when touching auth, data, APIs, user input):
  - Produce STRIDE threat model
  - Identify assets, entry points, threats, mitigations
  - No custom cryptographic logic — use vetted libraries only

- Define test strategy for each acceptance criterion
- Identify rollback path
- For prompt changes: note current vs. expected output differences

**Gate:** Present plan to user for non-trivial changes.

---

### 9. WRITE TESTS

**Goal:** Encode acceptance criteria as executable tests BEFORE implementation. Tests must fail first.

**Applies to:** Standard and Complex. Quick fix (skip — verify manually).

Actions:
- For each acceptance criterion, write a test
- Tests MUST FAIL before implementation (red phase)
- **Include failure-mode tests** (Chaos Engineering): What happens when the dependency is unavailable? When input is malformed? When the datastore is slow?
- For refactoring: existing tests must PASS before changes begin (proving current behavior), then continue passing after

**Gate:** Tests must exist and fail before proceeding to step 10.

---

### 10. IMPLEMENT

**Goal:** Write code that follows existing patterns and makes failing tests pass.

**Applies to:** All tiers.

Actions:
- Write the code change
- **Target: make tests from step 9 pass**

- **PATTERN CONFORMANCE CHECK:**
  - Grep codebase for how same operation is done elsewhere
  - Follow existing pattern (if it works) or fix all instances (if broken)
  - New patterns → note for archive

- **SCOPE COMPLETENESS CHECK:**
  - Grep every reference to changed code
  - Check all call sites, all usages, retry/fallback paths
  - Check read AND write paths for data model changes

- **ANDON CORD — HARD STOP CONDITIONS** (Toyota/Lean):
  These are blockers, not warnings. If any occur, STOP and fix before continuing:
  - Failing tests (that weren't already failing)
  - Build errors
  - SAST/security findings at medium or higher severity
  - Type errors
  - Introducing a pattern that contradicts an archived invariant

- **Feature flags** (Trunk-Based Development): For incomplete features landing in trunk, use flags. Document flag purpose, owner, and expected removal date.

---

### 11. VERIFY

**Goal:** Confirm the code works with staged confidence building.

**Applies to:** All tiers.

Actions:
- Build (debug + release)
- **DEPLOYMENT VERIFICATION:**
  - Binary timestamp newer than source change
  - Service fully started before testing
  - Process running the new artifact, not cached old one
- Run tests from step 9 — all must pass (green phase)
- For DB changes: verify with direct query, not just application layer
- For API/tool changes: call endpoint and verify output

- **OPERATIONAL READINESS REVIEW** (Amazon ORR, for deployable changes):
  - [ ] Error paths handled and surfaced to user
  - [ ] Rollback mechanism exists and is documented
  - [ ] Observability: can you tell if this is working or broken in production?
  - [ ] Documentation updated
  - [ ] No hardcoded secrets, credentials, or environment-specific values

- **SELF-REVIEW CHECKLIST** (Google Engineering Practices):
  Before presenting to user, self-check:
  - [ ] Design: Does this code belong? Is the architecture sound?
  - [ ] Functionality: Edge cases handled? Concurrency safe?
  - [ ] Complexity: Can a reader understand this quickly? Any over-engineering?
  - [ ] Naming: Clear and communicative?
  - [ ] Comments: Explain *why*, not *what*?
  - [ ] Tests: Test behavior, not implementation details?
  - [ ] Non-goals: Does this stay within declared scope?

**If verification fails:** Proceed to step 12 (Debug).

---

### 12. DEBUG

**Goal:** Find root cause. Record dead ends in real time.

**Applies to:** All tiers (when verification fails).

Actions:
- **Before investigating:** Check archive and postmortem repository for known dead ends
- Find root cause — read errors, check assumptions, focused fixes
- **Record dead ends in real time** — don't wait until fixed
- **Check layer mismatch:** Is the symptom in the same layer as the cause? Rendering bugs can have data-layer root causes
- **Five Whys** (Amazon COE): Ask "why did this happen?" iteratively until you reach a systemic cause, not just a proximate cause
- If stuck: escalate with dead ends documented

**On fix found:** Return to step 10. Fix goes through same checks.

---

### 13. REQUIREMENTS VERIFICATION

**Goal:** Confirm the change delivers what the user asked for, not just that it "works."

**Applies to:** Standard and Complex. Quick fix (skip).

Actions:
- Walk each acceptance criterion from the **spec artifact**, not conversation memory
- **Evidence over assertions** (spec-then-code): Show output, test results, direct queries — not "I believe this works"
- Ask: "Does this match what you expected?"
- If any criterion isn't met: return to step 10

---

### 14. FRESH-AGENT REVIEW

**Goal:** Catch rationalizations from an implementation-biased agent.

**Applies to:** Complex (mandatory). Standard (optional for high-risk). Quick fix (skip).

Actions:
- New session or subagent with NO conversation history
- Provide ONLY: spec artifact, code diff, relevant archive entries
- Reviewer task: "Compare diff against spec. Flag spec violations, missing criteria, unhandled edge cases, architectural concerns."
- Address findings before proceeding

---

### 15. ARCHIVE CAPTURE

**Goal:** Preserve decisions, constraints, lessons, and process insights.

**Applies to:** All tiers (depth varies).

Write entries for:

| Trigger | What to Capture |
|---------|----------------|
| Design decision with non-obvious rationale | INVARIANT + WHY + WHAT BREAKS |
| Bug fix where root cause was non-obvious | INVARIANT + WHY + DEAD ENDS |
| Dead end during debugging | DEAD ENDS (what failed + why) |
| Constraint discovered through failure | INVARIANT + WHY + WHAT BREAKS |
| Prompt/LLM behavior change | INVARIANT + WHY + WHAT BREAKS |
| New pattern introduced | Pattern + WHY this pattern |
| Existing entry affected | UPDATE or RETIRE |

**PROCESS REFLECTION** (mandatory Complex, recommended Standard):
- **What worked:** One thing the process did well
- **What caused friction:** One thing that slowed you down
- **Process improvement:** Specific change to this framework

Append reflections to `docs/archive/process-retrospective.md`.

**Update spec artifact** status to "complete." Update design discussion status to "implemented."

---

### 16. DONE

Checklist:
- [ ] Complexity tier assessed; appropriate steps followed
- [ ] All spec acceptance criteria met and confirmed by user
- [ ] Tests pass
- [ ] Fresh-agent review completed (Complex) or waived
- [ ] Archive updated (decisions, dead ends, alternatives, prompt rationale)
- [ ] Process reflection recorded
- [ ] Stale archive entries pruned
- [ ] Spec and design discussion statuses updated
- [ ] No known issues deferred without user acknowledgment

---

## Triggered Processes

### COE / Postmortem (triggered by production failures)

When agent-generated code causes rollback, user-visible failure, or security issue:

1. **Document:** What happened, when, what was the impact
2. **Timeline:** Code introduction → Detection → Escalation → Mitigation → Remediation → Close (with timestamps)
3. **Five Whys:** Iterate "why did this happen?" to systemic root cause
4. **Action Items:** Each must map to a concrete change:
   - New archive entry (invariant or dead end)
   - New checklist item in ORR or self-review
   - Updated constraint in framework
   - New test case
5. **Feedback loop** (Amazon COE → ORR): Ask "Would any existing archive entry or checklist item have prevented this?" If yes, why wasn't it consulted? If no, create one.

See `templates/coe-postmortem.md`.

### Error Budget Review (periodic)

Track change failure rate for agent-generated code:
- **Target:** < 5% of changes require rollback within 24 hours
- When budget is exhausted: increase human review requirements, reduce agent autonomy
- When budget is healthy: maintain current process

---

## Principles (RUF-derived)

Four principles underpin the entire framework:

1. **Accountability** — Every task, decision, and deliverable has one named person accountable. Not a team, not a role — a specific person who agreed to the responsibility.
2. **Transparency** — "Done" is defined in writing before work begins. Ambiguous language ("we'll make progress") is the primary vector for failure.
3. **Integrity** — Track actual delivery against commitments. Measure follow-through, not intentions.
4. **Commitment** — Move from "I'll try" to "It will be so." Rigorous commitments made with full awareness of risks and constraints.

---

## Agent Team Structure

How to structure agents for each step of the process. Based on empirical research from Anthropic's multi-agent system, MetaGPT (ICLR 2024), Agyn (SWE-bench 72.2%), and production analysis of Cursor, Windsurf, and Copilot.

### Core Findings

**Single-agent with good context management outperforms multi-agent orchestration for interactive development.** Cursor, Windsurf, and Copilot all converged on single-agent architectures. Multi-agent pays off only for genuinely independent parallel tasks, not for coordination-heavy sequential work.

**Agents communicate through documents, not conversations.** MetaGPT scored 3.9 vs ChatDev's 2.1 — the gap is almost entirely explained by structured artifact exchange vs conversational agent chains. Specs, ADRs, and review reports are the communication medium.

**Fresh context per implementation unit.** Each story/feature/PR starts with a clean context window loaded from spec + architecture + archive only. Accumulated context from prior stories introduces bias and degrades quality.

**The minimum number of agents for genuine parallelism.** Anthropic's system performed worse with 50 subagents than with 3-5. Three focused agents outperform one generalist only when tasks are genuinely independent.

### Agent Assignment Per Step

| Step | Agents | Model Tier | Why |
|------|--------|-----------|-----|
| 1-4 (Design → Spec) | Single + user | Reasoning (Opus) | Design requires unified context + human judgment |
| 5 (Explore) | 1-3 parallel subagents | Standard (Sonnet) | Independent file/module exploration parallelizes well |
| 6 (Read Archive) | Single | Standard | Sequential, small scope |
| 7 (Impact Analysis) | 1-2 subagents | Standard | Caller/callee tracing + schema analysis can parallelize |
| 8 (Plan) | Single | Reasoning (Opus) | Planning needs unified context; use strongest model |
| 9 (Write Tests) | 1-3 parallel subagents | Standard | Independent test files parallelize |
| 10 (Implement) | Single per story | Standard | Fresh context per unit; no accumulation across stories |
| 11 (Verify) | Single | Standard | Sequential build/deploy/test |
| 12 (Debug) | Single | Reasoning (Opus) | Root cause analysis needs unified context |
| 13 (Req Verification) | Single + user | Standard | Human judgment required |
| 14 (Fresh Review) | **New agent, mandatory** | Standard | Zero conversation history; adversarial framing |
| 15 (Archive) | Single | Standard | Sequential documentation |

### When to Add Subagents

Add subagents ONLY when:
- Tasks are genuinely independent (no shared state, no shared files)
- Each subagent's work doesn't need the other's output
- The parallelism saves more time than the coordination costs

Do NOT add subagents when:
- Tasks modify the same files (merge conflicts)
- Step 2's output is step 3's input (sequential dependency)
- The task is small enough that delegation overhead exceeds work time
- You need unified reasoning across all the information

### Model Tiering

Use the strongest available model for steps requiring deep reasoning:
- **Reasoning model** (Opus-class): Steps 1-4 (design), 8 (plan), 12 (debug)
- **Standard model** (Sonnet-class): Steps 5-7 (explore/analyze), 9-11 (test/implement/verify), 13-15 (review/archive)
- **Fast model** (Haiku-class): Mechanical extraction tasks within pipelines (not framework steps)

This is cost optimization through capability matching, not quality compromise. Agyn achieved 72.2% SWE-bench using GPT-5 for orchestration and cheaper models for implementation.

---

## Agent Prompting: What Works and What Doesn't

Research on persona/role prompting reveals critical findings for how agents are instructed within each step.

### What the Evidence Shows

**Persona framing ("you are a senior security engineer") DEGRADES coding and factual accuracy:**
- Expert personas on MMLU: -22 percentage points (Llama-3.1-8B)
- Expert personas on MT-Bench coding: -0.65 points
- Mechanism: persona prompting redirects attention away from the model's actual knowledge

**What DOES improve output quality:**
- **Domain checklists** — listing specific categories to check (OWASP Top 10, STRIDE categories)
- **Methodology scaffolding** — plan→implement→verify phases (implicit chain-of-thought)
- **Tool restrictions** — read-only for reviewers, write for implementers (architectural constraint)
- **Explicit I/O specifications** — concrete inputs, expected outputs, edge cases
- **Stepwise breakdowns** — decomposing complex tasks into ordered sub-steps

### The Optimal Prompt Structure

**Do this:**
```
Review this code for OWASP Top 10 vulnerabilities:
- A01: Broken Access Control — check authorization on all endpoints
- A02: Cryptographic Failures — check key management, algorithm choices
- A03: Injection — check all user input paths for SQL/command injection
[...specific checklist continues...]

For each finding: state the category, quote the vulnerable code, explain the risk, provide the fix.
```

**Not this:**
```
You are a senior security auditor with 15 years of penetration testing
experience specializing in web application security...
```

The first injects a checklist that activates specific knowledge. The second activates a persona that displaces knowledge. Research shows the first outperforms.

### Prompting Rules for This Framework

1. **No persona framing.** Do not tell the agent "you are an expert in X." Instead, provide the domain checklist, methodology, and evaluation criteria for X.
2. **Inject domain knowledge, not domain identity.** List the specific patterns to check, categories to evaluate, criteria to apply. The model already has the knowledge — activate it with specifics, not personas.
3. **Methodology over personality.** Structure prompts as: context → task → checklist → output format. Not: identity → backstory → task.
4. **Tool permissions enforce roles.** A reviewer is a reviewer because it has read-only tools, not because it's told "you are a reviewer."
5. **Specificity plateau at ~100 words.** Performance gains from prompt detail plateau around 100 words and decline beyond 200. Keep task descriptions focused.

### Applying to Fresh-Agent Review (Step 14)

The adversarial review framing is the one place where role framing has empirical support — not for accuracy, but for behavioral alignment. BMAD's finding: reviews with zero findings should trigger re-analysis.

**Effective review prompt:**
```
Compare this diff against the spec below. Your task is to find issues.

Checklist:
- [ ] Every acceptance criterion in the spec is addressed by the diff
- [ ] No code changes fall outside the declared scope/non-goals
- [ ] Edge cases from the pre-mortem are handled or documented as assumptions
- [ ] No archived invariants are violated
- [ ] Security: no hardcoded secrets, no injection paths, no auth bypasses

You MUST find and report at least one concern, question, or suggestion.
If you find zero issues, re-examine — zero findings indicates insufficient review depth.

SPEC:
[spec artifact]

DIFF:
[code diff]

ARCHIVE ENTRIES:
[relevant archive entries]
```
