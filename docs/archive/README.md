# Development Archive

Living record of design decisions, constraints, invariants, and hard-won debugging lessons. This archive exists so that AI agents working on this codebase can understand **why** things are the way they are, not just what they are.

## Purpose

Code shows WHAT. Commits show WHAT CHANGED. This archive shows **WHY this and not the obvious alternative, and WHAT BREAKS if you change it.**

## Organization

Archives are split by module. Create a new file when a module accumulates 5+ entries:

```
docs/archive/
  README.md                    # This file
  process-retrospective.md     # Process reflection log
  <module-name>.md             # Per-module decision records
```

Name archive files after the module or domain they cover (e.g., `auth.md`, `database.md`, `api.md`, `deployment.md`).

## Entry Format

See `templates/archive-entry.md` for the full format specification.

## Rules

1. **Update on change.** If you modify code that an archive entry describes, update or retire the entry in the same change.
2. **No implementation details.** Capture constraints and rationale, not code. Code changes; invariants persist.
3. **Dead entries are dangerous.** A stale entry that says "we use pattern X" when the code uses pattern Y will actively mislead. Prune aggressively.
4. **Keep entries scannable.** An agent should find the relevant entry in <10 seconds. If an archive file exceeds ~80 entries, split it.
