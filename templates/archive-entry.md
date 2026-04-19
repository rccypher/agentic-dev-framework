# Archive Entry Template

Use this format for each entry in `docs/archive/<module-name>.md`.

```markdown
### [Short Title] (YYYY-MM-DD)

**INVARIANT:** What must remain true.

**WHY:** The reasoning — what problem this solves or what failure led to this.

**WHAT BREAKS:** Specific consequences of violating the invariant.

**DEAD ENDS:** (optional) Approaches tried and failed, and why.

**ALTERNATIVES CONSIDERED:** (optional) Other approaches evaluated and why they were rejected.

**RELATED FILES:** File paths and line ranges.
```

## Field Guidelines

- **INVARIANT:** State the constraint, not the implementation. "Never use pattern X for operation Y" is better than "line 42 does Z." Code changes; invariants persist.
- **WHY:** Include the specific failure or scenario that led to this. "Caused a 20-minute debugging session" is more persuasive than "this is best practice."
- **WHAT BREAKS:** Be specific. "Users see wrong values" not "things break." Future agents need to assess whether the invariant still applies.
- **DEAD ENDS:** Write these during debugging (step 12), not after. Include WHY the approach failed, not just that it did. "X didn't work because Y returns Int64 not Int" is useful. "X didn't work" is not.
- **ALTERNATIVES CONSIDERED:** Explain what was rejected and why. This prevents future agents from rediscovering the same dead end as a "new idea."
- **RELATED FILES:** Include line ranges so agents can find the relevant code quickly.

## When to Write an Entry

| Trigger | Required Fields |
|---------|----------------|
| Design decision with non-obvious rationale | INVARIANT + WHY + WHAT BREAKS |
| Bug fix where root cause was non-obvious | INVARIANT + WHY + DEAD ENDS |
| Dead end encountered during debugging | DEAD ENDS (what failed + why) |
| Constraint discovered through failure | INVARIANT + WHY + WHAT BREAKS |
| Prompt/LLM behavior change | INVARIANT + WHY + WHAT BREAKS |
| New pattern introduced to the codebase | Pattern description + WHY this pattern |

## Maintenance Rules

- **Update on change:** If you modify code that an archive entry describes, update the entry in the same change.
- **No stale entries:** A wrong entry is worse than no entry. Prune when you find outdated information.
- **Constraints, not code:** Capture invariants and rationale, not implementation details.
- **Split when large:** If an archive file exceeds ~80 entries, split by logical submodule.
