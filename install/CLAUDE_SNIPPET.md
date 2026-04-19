# CLAUDE.md Snippet

Add this to your project's `CLAUDE.md` (or equivalent agent configuration file).

---

## For project-level CLAUDE.md

```markdown
# Mandatory Development Process

**ALL development work MUST follow the process defined in `Docs/DEVELOPMENT_PROCESS.md`.**

The process has 16 steps across 3 complexity tiers (Quick Fix / Standard / Complex). Key gates:

1. **Complexity Assessment** — route to appropriate tier; set time budget; when in doubt, go one tier up
2. **Design Discussion Log** — run the Discovery Process (Docs/DISCOVERY.md) for structured elicitation; record to Docs/design-discussions/
3. **Requirements Confirmation** — restate task, acceptance criteria, scope boundaries; user confirms before any code is read
4. **Spec Artifact** — generate Docs/specs/*.spec.md with non-goals, pre-mortem, and STRIDE security assessment
6. **Read Archive** — check Docs/archive/ for invariants, dead ends, constraints, and postmortems
7. **Impact Analysis** — blast radius, risk storming, riskiest assumption test
8. **Plan** — approach + alternatives considered + pre-mortem + test strategy + rollback path
9. **Write Tests** — failing tests against acceptance criteria BEFORE implementation
10. **Implement** — pattern conformance, scope completeness, Andon stops (hard stop on quality failures)
11. **Verify** — ORR checklist, self-review checklist, tests green, deployment verification
13. **Requirements Verification** — walk spec criteria with user, evidence over assertions
14. **Fresh-Agent Review** — new session reviews spec + diff with zero conversation history (Complex tier)
15. **Archive Capture** — decisions, constraints, dead ends, prompt rationale, process reflection

**Do not skip steps for your tier. Do not start coding before confirming requirements. Do not present work as done before verifying acceptance criteria.**

When agent-generated code causes rollback or user-visible failure, trigger the COE/Postmortem process (templates/coe-postmortem.md).
```

## For global agent config (e.g., ~/.claude/CLAUDE.md)

```markdown
# Mandatory Development Process — ALL PROJECTS

All code development work MUST follow the 16-step agentic development process.
Canonical framework: https://github.com/rccypher/agentic-dev-framework
Project-specific instance: each project's Docs/DEVELOPMENT_PROCESS.md

For projects without a development process file: initialize from the framework repo.

**Do not start coding before confirming requirements. Do not present work as done before verifying acceptance criteria with the user.**
```
