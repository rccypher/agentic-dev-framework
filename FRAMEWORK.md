# Agentic Development Framework

**Version:** 1.0.0 (2026-04-17)

A 16-step structured development process designed for AI coding agents. Built from real-world failures and validated against BMAD, GitHub Spec Kit, Amazon Kiro, OpenSpec, RIPER-5, Memory Bank, ADRs, spec-then-code, and Aider methodologies.

This framework solves the core problem of AI-assisted development: **agents lose context, repeat mistakes, skip verification, and produce code that works but doesn't match what was asked for.** Every step exists because its absence caused a specific, documented failure.

---

## Complexity Routing

Not every change needs the full process. Assess complexity first and follow the appropriate tier.

| Tier | Criteria | Steps |
|------|----------|-------|
| **Quick Fix** | Single function/file, no schema change, obvious root cause, no new patterns | 5 → 10 → 11 → 15 (brief) → 16 |
| **Standard** | 2-3 files, clear scope, no architectural changes | 3 → 4 → 5 → 6 → 8 → 9 → 10 → 11 → 13 → 15 → 16 |
| **Complex** | 3+ modules, schema/API changes, new patterns, prompt engineering, architectural decisions | All 16 steps |

**When in doubt, go one tier up.** Underestimating complexity and skipping steps costs more than the extra time on a higher tier. If during any step you realize the change is more complex than initially assessed, escalate to the higher tier and backfill skipped steps.

---

## Process Overview

```
TASK RECEIVED
     |
     v
1. COMPLEXITY ASSESSMENT -----> Route to Quick Fix / Standard / Complex
     |
     v
2. DESIGN DISCUSSION LOG -----> Structured elicitation, record to file
     |
     v
3. REQUIREMENTS CONFIRMATION -----> User confirms or corrects
     |
     v
4. SPEC ARTIFACT -----> Generate formal spec as source of truth
     |
     v
5. EXPLORE (read relevant code, identify affected modules)
     |
     v
6. READ ARCHIVE (check invariants, dead ends, constraints)
     |
     v
7. IMPACT ANALYSIS -----> If blast radius > 3 modules, confirm with user
     |
     v
8. PLAN (approach + alternatives considered + test strategy + rollback)
     |                                          |
     v                                User confirms if non-trivial
9. WRITE TESTS (failing tests against acceptance criteria)
     |
     v
10. IMPLEMENT (with pattern conformance + scope completeness checks)
     |
     v
11. VERIFY (build, deploy, run tests, direct data validation)
     |
     v
  PASS? --NO--> 12. DEBUG (root cause, record dead ends) --> return to 10
     |
    YES
     |
     v
13. REQUIREMENTS VERIFICATION (walk each criterion with user, show evidence)
     |
     v
  ALL MET? --NO--> return to 10
     |
    YES
     |
     v
14. FRESH-AGENT REVIEW (new session reviews spec + diff, no conversation history)
     |
     v
15. ARCHIVE CAPTURE (decisions, constraints, dead ends, prompt rationale, process reflection)
     |
     v
16. DONE
```

---

## Step Details

### 1. COMPLEXITY ASSESSMENT

**Goal:** Route the task to the appropriate process tier to match effort to risk.

**Applies to:** All tiers (this step determines the tier).

Actions:
- Assess the change along these dimensions:
  - **Files affected:** 1 file (quick fix) | 2-3 files (standard) | 3+ files (complex)
  - **Schema/API changes:** No (quick fix or standard) | Yes (complex)
  - **New patterns introduced:** No (quick fix or standard) | Yes (complex)
  - **Architectural impact:** None (quick fix) | Local (standard) | Cross-module (complex)
  - **Risk if wrong:** Low/easily reverted (quick fix) | Moderate (standard) | High/hard to revert (complex)
- State the assessed tier explicitly: "This is a [quick fix / standard / complex] change because [reason]."
- If the user disagrees with the assessment, adjust.

**Escalation rule:** If during ANY subsequent step you discover the change is more complex than assessed, stop and escalate to the higher tier. Backfill any skipped steps before continuing.

**Origin:** Adapted from Memory Bank's complexity levels (1-4) and spec-then-code's risk/complexity matrix. The insight: uniform process overhead on trivial changes creates friction that erodes adoption, but undertriaging causes more damage than overtriaging.

---

### 2. DESIGN DISCUSSION LOG

**Goal:** Capture the initial conversation through structured elicitation before anything is formalized.

**Applies to:** Complex tier (mandatory). Standard tier (optional — create if discussion is substantive). Quick fix (skip).

Actions:
- Create a log file at `docs/design-discussions/YYYY-MM-DD-<short-slug>.md`
- **Structured elicitation** — actively ask targeted questions to surface hidden requirements:
  - Edge cases: "What happens when [input is empty / API is down / quota is exceeded]?"
  - Error conditions: "How should failures be surfaced to the user?"
  - Performance constraints: "Are there latency or throughput requirements?"
  - Security implications: "Does this touch credentials, user data, or external APIs?"
  - Rollback scenarios: "If this breaks something in production, what's the recovery path?"
  - User experience: "What should the output look like? Can you describe the ideal result?"
- Record the conversation as it happens: what the user asked for, what approaches were considered, what tradeoffs were discussed, what was rejected and why
- Include the user's original words — paraphrase loses nuance and intent

See `templates/design-discussion.md` for the file template.

**Origin:** Combines Claude Code's "Let Claude interview you" pattern with BMAD's "Advanced Elicitation." The insight: conversations that shape design decisions vanish when the context window compacts. The archive captures constraints AFTER implementation, but the reasoning that led to the approach is lost without this step.

---

### 3. REQUIREMENTS CONFIRMATION

**Goal:** Eliminate ambiguity before any code is read or written.

**Applies to:** Standard and Complex tiers. Quick fix (skip — scope is self-evident).

Actions:
- Restate the task in your own words
- List explicit acceptance criteria: "This is done when X, Y, Z"
- List scope boundaries: "This does NOT include A, B"
- Identify unknowns or assumptions that need confirmation
- If the task involves user-facing output, confirm what the output should look like

**Gate:** Do not proceed until the user confirms the requirements and acceptance criteria. If the user says "just do it" without confirming, restate your understanding in one sentence and proceed — but flag any assumptions you're making.

**Origin:** No other surveyed framework has an explicit "these things will NOT be done in this task" confirmation gate. This prevents scope creep and forces the agent to declare its assumptions before investing effort.

---

### 4. SPEC ARTIFACT

**Goal:** Produce a formal, versioned specification that serves as the single source of truth for the entire lifecycle of this change.

**Applies to:** Standard and Complex tiers. Quick fix (skip).

Actions:
- Generate `docs/specs/YYYY-MM-DD-<short-slug>.spec.md` using the template
- The spec is a **living document** — update it when requirements change rather than relying on conversation history
- Step 13 (Requirements Verification) walks against THIS document, not conversation memory
- If requirements change mid-implementation, update the spec FIRST, get user confirmation, then continue

See `templates/spec.md` for the file template.

**Origin:** GitHub Spec Kit, Amazon Kiro, and OpenSpec all converge on the same insight: when the specification is a persistent artifact rather than a conversation, requirements don't drift. The spec becomes the anchor that all subsequent steps reference.

---

### 5. EXPLORE

**Goal:** Understand what exists before changing anything.

**Applies to:** All tiers.

Actions:
- Read the relevant source files — do not propose changes to code you haven't read
- Understand current behavior: what does the code do today?
- Identify all modules/files that will be affected
- Note existing patterns: how does similar code in this project handle the same problem?

---

### 6. READ ARCHIVE

**Goal:** Learn from previous decisions and failures before repeating them.

**Applies to:** Standard and Complex tiers. Quick fix (check archive only if the fix touches a module with known gotchas).

Actions:
- Open the archive file(s) in `docs/archive/` for each affected module
- Check invariants: are any constraints documented that affect your planned approach?
- Check dead ends: has the approach you're considering already been tried and failed?
- Check constraints: are there platform quirks, API limits, or type system gotchas documented?

If no archive file exists for the affected module yet, skip this step but create one in step 15.

**Origin:** Unique to this framework. ADRs document accepted decisions, but not failed approaches. Memory Bank's systemPatterns.md organizes by pattern type, not by "things that failed." This step specifically consults known failures to avoid repeating them.

---

### 7. IMPACT ANALYSIS

**Goal:** Understand the blast radius before making changes. Prevent collateral damage.

**Applies to:** Standard and Complex tiers. Quick fix (skip).

Actions:
- List every file that will be modified
- List every file that CALLS or IS CALLED BY the modified code
- For DB changes: check schema constraints (UNIQUE, NOT NULL, foreign keys, triggers)
- Grep for shared patterns: if changing a pattern in one file, search for the same pattern elsewhere
- Assess blast radius: how many modules does this touch?

**Gate:** If blast radius > 3 modules, present the impact analysis to the user and get confirmation before proceeding.

**Origin:** Unique to this framework. Other frameworks discuss architecture but none have a specific step that traces dependency chains and schema constraints before any code is written.

---

### 8. PLAN

**Goal:** Design the approach, document alternatives, define how to test it, and identify rollback.

**Applies to:** Standard and Complex tiers. Quick fix (skip).

Actions:
- Design the implementation approach
- Verify the plan does NOT violate any archived invariants from step 6

- **ALTERNATIVES CONSIDERED** (mandatory for Complex, recommended for Standard):
  - Document at least one alternative approach that was considered
  - For each alternative: why it was rejected (performance, complexity, risk, constraint violation)
  - Record this in the spec artifact under "Alternatives Considered"

- Define test strategy for each acceptance criterion:
  - What input to test with
  - What output to expect
  - What failure looks like
  - How to verify at the data layer — don't trust only the application layer
- Identify rollback path: if this breaks something, how do we undo it?
- For prompt/LLM changes: note what the current behavior is and what should change

**Gate:** For non-trivial changes, present the plan to the user for confirmation.

**Origin:** Alternatives Considered adapted from ADR format and Memory Bank's /creative phase. Rollback path planning is unique to this framework.

---

### 9. WRITE TESTS

**Goal:** Encode acceptance criteria as executable tests BEFORE writing any implementation code. Tests must fail before implementation begins.

**Applies to:** Standard and Complex tiers. Quick fix (skip — verify manually in step 11).

Actions:
- For each acceptance criterion, write a test that verifies it
- Tests MUST FAIL before implementation (red phase)
- Test types in order of preference:
  1. **Unit test** — fastest feedback, most precise
  2. **Integration test** — tests cross-module behavior
  3. **Manual test script** — documented steps with expected output
  4. **Direct validation query** — SQL query, log grep, or API call with expected result
- If the project lacks test infrastructure for the affected module, write a manual test script with exact steps and expected results

**Gate:** Tests must exist and fail before proceeding to step 10.

**Origin:** Adapted from spec-then-code, GitHub Spec Kit constitutional articles, and cc-sdd. The insight: writing tests before implementation makes acceptance criteria executable. You know EXACTLY when you're done because the tests turn green.

---

### 10. IMPLEMENT

**Goal:** Write code that follows existing patterns and covers all affected code paths. Make the failing tests pass.

**Applies to:** All tiers.

Actions:
- Write the code change
- **Target: make the tests from step 9 pass** (Standard/Complex tiers)

- **PATTERN CONFORMANCE CHECK:**
  - Before writing a new pattern, grep the codebase for how the same operation is done elsewhere
  - If existing code uses a different pattern: follow it (if it works) or fix all instances (if it's broken)
  - If introducing a new pattern: note it for archive capture in step 15

- **SCOPE COMPLETENESS CHECK:**
  - Grep for every reference to the thing you changed
  - If you changed a function signature: check every call site
  - If you changed a constant or config value: check every usage
  - If you changed a prompt: check retry paths, fallback paths
  - If you changed a data model: check all read paths AND write paths

---

### 11. VERIFY

**Goal:** Confirm the code works before presenting results to the user.

**Applies to:** All tiers.

Actions:
- Build: does it compile?
- **DEPLOYMENT VERIFICATION:**
  - Verify the deployed artifact is newer than the source change (don't test against stale builds)
  - Verify the service is fully started before testing
- **Run tests from step 9** — all must pass (green phase)
- Execute any additional manual verification from the test strategy
- For DB changes: verify with direct query — don't trust only the application layer's rendering
- For API/tool changes: call the endpoint and verify the output matches expectations

**If verification fails:** Proceed to step 12 (Debug).

---

### 12. DEBUG

**Goal:** Find the root cause, not just a workaround. Record dead ends as you go.

**Applies to:** All tiers (when verification fails).

Actions:
- Find the root cause — read errors, check assumptions, try focused fixes
- **Before investigating:** Check the archive for known dead ends. If your intended approach is documented as a dead end, skip it
- **Record dead ends in real time:** Don't wait until the bug is fixed. As you try approaches that fail, note them immediately. These become archive entries in step 15
- Check: is the symptom in the same layer as the cause? Rendering bugs can have data-layer root causes. Network errors can have auth-layer root causes. Don't assume the symptom points to the source
- If stuck after investigation: escalate to the user WITH the dead ends documented

**On fix found:** Return to step 10 (Implement). The fix goes through the same pattern conformance and scope completeness checks.

**Origin:** Dead-end recording during debug is unique to this framework. Every other surveyed methodology documents decisions but not failed approaches during active debugging.

---

### 13. REQUIREMENTS VERIFICATION

**Goal:** Confirm the change delivers what the user asked for, not just that it "works."

**Applies to:** Standard and Complex tiers. Quick fix (skip — verification in step 11 is sufficient).

Actions:
- Walk through each acceptance criterion from the **spec artifact** (step 4), not from conversation memory
- For each criterion: show the user evidence that it's met (tool output, test result, direct query, screenshot)
- **Evidence over assertions:** "the test passes" or "here is the output" — not "I believe this works"
- Ask: "Does this match what you expected?"
- If any criterion isn't met: identify what's missing and return to step 10

**Origin:** Adapted from spec-then-code's "evidence over assertions" principle. Walking against the spec artifact rather than conversation memory prevents the agent from verifying against its own interpretation rather than the user's intent.

---

### 14. FRESH-AGENT REVIEW

**Goal:** Catch implementation rationalizations by having a reviewer with zero conversation history evaluate the change.

**Applies to:** Complex tier (mandatory). Standard tier (optional — use for high-risk changes). Quick fix (skip).

Actions:
- Start a new agent session (or use a subagent) with NO access to the current conversation history
- Provide the reviewer with ONLY:
  1. The spec artifact from step 4
  2. The code diff (all files changed)
  3. The relevant archive entries
- The reviewer's task: "Compare this diff against the spec. Flag any spec violations, missing acceptance criteria, unhandled edge cases, or architectural concerns."
- The reviewer must NOT have access to the implementation conversation
- Address any findings before proceeding

**Origin:** Adapted from OpenSpec's optional Review phase and RIPER-5's REVIEW phase. The structural insight: an agent that participated in implementation will rationalize its own decisions. A fresh agent with only the spec and diff evaluates objectively.

---

### 15. ARCHIVE CAPTURE

**Goal:** Preserve decisions, constraints, lessons, and process insights for future agents.

**Applies to:** All tiers (depth varies by tier).

For each of the following, ask "Did this happen?" — if YES, write an archive entry:

| Trigger | What to Capture |
|---------|----------------|
| Design decision with non-obvious rationale | INVARIANT + WHY + WHAT BREAKS |
| Bug fix where root cause was non-obvious | INVARIANT + WHY + DEAD ENDS |
| Dead end encountered during debugging | DEAD ENDS (what failed + why) |
| Constraint discovered through failure | INVARIANT + WHY + WHAT BREAKS |
| Prompt/LLM behavior change | INVARIANT + WHY + WHAT BREAKS |
| Existing archive entry affected by this change | UPDATE or RETIRE the entry |
| New pattern introduced to the codebase | Pattern description + WHY this pattern |

See `templates/archive-entry.md` for the entry format.

**PROCESS REFLECTION** (mandatory for Complex, recommended for Standard):

After capturing technical entries, add a brief process reflection:
- **What worked:** One thing the development process did well on this task.
- **What caused friction:** One thing that slowed you down or caused rework.
- **Process improvement suggestion:** (optional) A specific change to this framework that would help.

Append process reflections to `docs/archive/process-retrospective.md`.

Rules:
- Update on change: if you modified code that an archive entry describes, update the entry
- No stale entries: wrong entries actively mislead. Prune aggressively
- Constraints, not code: capture invariants and rationale. Code changes; reasons persist
- Split when large: if an archive file exceeds ~80 entries, split by submodule

**Origin:** Combines ADR format (INVARIANT/WHY/ALTERNATIVES) with unique additions: dead-end recording, prompt rationale capture, and process reflection (from Memory Bank's /reflect phase).

---

### 16. DONE

Checklist:
- [ ] Complexity tier was assessed and appropriate steps were followed
- [ ] All acceptance criteria from the spec artifact are met and confirmed by user
- [ ] Tests pass (automated or manual verification documented)
- [ ] Fresh-agent review completed (Complex tier) or waived (Standard/Quick fix)
- [ ] Archive updated with relevant decisions, constraints, dead ends, and alternatives
- [ ] Process reflection recorded (Complex and Standard tiers)
- [ ] Stale archive entries pruned or updated
- [ ] Spec artifact status updated to "complete"
- [ ] Design discussion status updated to "implemented"
- [ ] No known issues deferred without user acknowledgment
