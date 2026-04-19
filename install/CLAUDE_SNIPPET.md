# CLAUDE.md Snippet

Add this to your project's `CLAUDE.md` (or equivalent agent configuration file) to make the framework mandatory.

---

## For project-level CLAUDE.md

```markdown
# Mandatory Development Process

**ALL development work MUST follow the process defined in `Docs/DEVELOPMENT_PROCESS.md`.**

The process has 16 steps across 3 complexity tiers (Quick Fix / Standard / Complex). Key gates:
1. **Complexity Assessment** — route to appropriate tier; when in doubt, go one tier up
2. **Design Discussion Log** — structured elicitation + record conversation to `Docs/design-discussions/`
3. **Requirements Confirmation** — restate task, acceptance criteria, scope boundaries; user confirms
4. **Spec Artifact** — generate formal `Docs/specs/*.spec.md` as the single source of truth
6. **Read Archive** — check `Docs/archive/` for invariants, dead ends, constraints
8. **Plan** — approach + alternatives considered + test strategy + rollback path
9. **Write Tests** — failing tests against acceptance criteria BEFORE implementation
13. **Requirements Verification** — walk spec criteria with user, evidence over assertions
14. **Fresh-Agent Review** — new session reviews spec + diff with zero conversation history (Complex tier)
15. **Archive Capture** — decisions, constraints, dead ends, prompt rationale, process reflection

**Do not skip steps for your tier. Do not start coding before confirming requirements. Do not present work as done before verifying acceptance criteria with the user.**
```

## For global agent config (e.g., ~/.claude/CLAUDE.md)

```markdown
# Mandatory Development Process — ALL PROJECTS

All code development work MUST follow the 16-step process defined in each project's `Docs/DEVELOPMENT_PROCESS.md`.

For projects without a development process file: initialize one from https://github.com/rccypher/agentic-dev-framework

**Do not start coding before confirming requirements. Do not present work as done before verifying acceptance criteria with the user.**
```
