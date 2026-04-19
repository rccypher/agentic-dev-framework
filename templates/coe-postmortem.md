# COE / Postmortem: [Incident Title]

**Date:** YYYY-MM-DD
**Severity:** critical | significant | minor
**Triggered by:** rollback | user-visible failure | security issue | emergency hotfix

## Summary

One paragraph: what happened, what was the impact, how was it resolved.

## Timeline

| Phase | Timestamp | Description |
|-------|-----------|-------------|
| **Code Introduced** | | When the problematic change entered the codebase |
| **Detection** | | When the problem was first noticed, and how |
| **Escalation** | | When it reached the right person/process |
| **Mitigation** | | When the immediate impact was stopped |
| **Remediation** | | When the full fix was in place |
| **Close** | | When all follow-ups were completed |

**Detection delay:** [time between introduction and detection]
**Detection mechanism:** [test failure | user report | monitoring alert | manual review | other]

## Impact

- Users affected:
- Data affected:
- Duration of impact:

## Five Whys

1. **Why did this happen?**
   → [proximate cause]

2. **Why did [proximate cause] occur?**
   →

3. **Why did [that] occur?**
   →

4. **Why did [that] occur?**
   →

5. **Why did [that] occur?**
   → [systemic root cause]

## What Information Did the Agent Have?

- What was in the spec/requirements?
- What constraints were documented in the archive?
- What tests existed?
- What was the agent's reasoning (if available from conversation history)?

## What Verification Layer Failed?

- [ ] Tests didn't cover this case
- [ ] Tests existed but didn't catch the issue
- [ ] Self-review checklist missed it
- [ ] Fresh-agent review missed it (or wasn't run)
- [ ] ORR checklist missed it
- [ ] Manual verification missed it
- [ ] No verification existed for this scenario

## Action Items

Each must map to a concrete change. Abstract recommendations are not acceptable.

| # | Action | Type | Owner | Status |
|---|--------|------|-------|--------|
| 1 | | archive entry / checklist item / new test / framework update / constraint | | |
| 2 | | | | |

## Feedback Loop Check (Amazon COE → ORR)

- Was the relevant archive consulted before this change? [ ] Yes [ ] No
- Would any existing archive entry have prevented this? [ ] Yes [ ] No → If yes, why wasn't it consulted?
- Would any existing checklist item have caught this? [ ] Yes [ ] No → If yes, why wasn't it checked?
- What new archive entry or checklist item has been created to prevent recurrence?
