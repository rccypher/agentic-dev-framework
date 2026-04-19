# Spec: [Feature/Change Name]

**Date:** YYYY-MM-DD
**Status:** draft | confirmed | implementing | complete | superseded
**Task Type:** feature | bug fix | refactoring | security fix | prompt engineering | infrastructure
**Complexity:** quick fix | standard | complex
**Time Budget:** [X hours/minutes]
**Supersedes:** (link to prior spec if replacing an earlier approach)
**Design Discussion:** (link to design-discussions/ file)

## Problem Statement

What problem this solves. Why it matters. One paragraph maximum.

## Acceptance Criteria

Numbered list. Each criterion is independently testable.

1.
2.
3.

## Scope

### In Scope

What this change includes.

-
-

### Out of Scope

What this change explicitly does NOT include.

-
-

### Non-Goals (Google Design Docs)

Explicit statements of what this implementation will NOT do — even if it might seem related or useful. These prevent scope creep and give the agent concrete boundaries to self-check against.

-
-

## Open Questions

Unresolved items that may affect implementation. Resolved during exploration/planning.

-

## Pre-Mortem (Gary Klein)

*"This implementation has already failed. What went wrong?"*

List every reason it could fail. Each becomes a test case or documented assumption.

1.
2.
3.

## Security Assessment (Microsoft SDL — STRIDE)

*Required when touching auth, data storage, external APIs, or user input. Delete this section if not applicable.*

| STRIDE Category | Threat | Mitigation |
|----------------|--------|------------|
| **S**poofing | | |
| **T**ampering | | |
| **R**epudiation | | |
| **I**nformation Disclosure | | |
| **D**enial of Service | | |
| **E**levation of Privilege | | |

## Technical Approach

(Filled in after step 8 — PLAN)

## Alternatives Considered

(Filled in after step 8 — PLAN. Document at least one rejected approach and why.)

### Alternative 1: [Name]

**Approach:**
**Why rejected:**

## Riskiest Assumption

(Filled in after step 7/8 — the single assumption most likely to cause failure)

**Assumption:**
**How to test cheaply:**
**Result:**

## Test Strategy

| Acceptance Criterion | Test Type | Input | Expected Output | Failure Mode Test |
|---------------------|-----------|-------|-----------------|-------------------|
| 1. | unit / integration / manual / query | | | |
| 2. | | | | |

## Rollback Plan

How to undo this change if it breaks something in production.

## Bug Fix Fields (delete if not a bug fix)

**Current behavior:**
**Expected behavior:**
**Behavior that must remain unchanged:**
