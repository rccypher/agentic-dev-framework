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

Every step in this framework exists because its absence caused a specific, documented failure.

## Quick Start

### 1. Add to your project

Copy the framework files into your project:

```bash
# Clone the framework
git clone https://github.com/rccypher/agentic-dev-framework.git

# Copy into your project
cp agentic-dev-framework/FRAMEWORK.md your-project/Docs/DEVELOPMENT_PROCESS.md
cp -r agentic-dev-framework/templates your-project/Docs/templates
mkdir -p your-project/Docs/{archive,design-discussions,specs}
```

### 2. Add to your CLAUDE.md (or equivalent agent config)

Add the snippet from `install/CLAUDE_SNIPPET.md` to your project's agent configuration file. This makes the process mandatory for all development work in that project.

### 3. Start using it

The framework routes tasks through 3 complexity tiers:

| Tier | When | Steps |
|------|------|-------|
| **Quick Fix** | Single file, obvious fix | 5 steps |
| **Standard** | 2-3 files, clear scope | 11 steps |
| **Complex** | 3+ modules, architectural | All 16 steps |

## The 16 Steps

```
 1. COMPLEXITY ASSESSMENT      Determine tier: quick fix / standard / complex
 2. DESIGN DISCUSSION LOG      Structured elicitation, record conversation
 3. REQUIREMENTS CONFIRMATION  Restate task, acceptance criteria, user confirms
 4. SPEC ARTIFACT              Formal spec.md as single source of truth
 5. EXPLORE                    Read code, identify affected modules
 6. READ ARCHIVE               Check invariants, dead ends, constraints
 7. IMPACT ANALYSIS            Blast radius, callers/callees, schema constraints
 8. PLAN                       Approach + alternatives + tests + rollback
 9. WRITE TESTS                Failing tests before implementation
10. IMPLEMENT                  Pattern conformance + scope completeness
11. VERIFY                     Build, deploy, tests green, data validation
12. DEBUG                      Root cause, record dead ends in real time
13. REQUIREMENTS VERIFICATION  Walk spec criteria with evidence
14. FRESH-AGENT REVIEW         New session reviews spec + diff
15. ARCHIVE CAPTURE            Decisions, dead ends, process reflection
16. DONE                       Checklist verification
```

See [FRAMEWORK.md](FRAMEWORK.md) for full step details.

## What Makes This Different

Compared to BMAD, GitHub Spec Kit, Amazon Kiro, OpenSpec, RIPER-5, Memory Bank, ADRs, spec-then-code, and Aider:

**Unique to this framework:**
- **Dead-end recording during debug** — no other framework documents failed approaches during active debugging
- **Prompt/LLM rationale capture** — records why a particular prompt was structured a certain way
- **Scope boundary confirmation** — explicit "this will NOT be done" gate
- **Pre-plan archive review** — consults known failures before planning
- **Blast radius analysis** — traces callers/callees/schema constraints before any code is written
- **Rollback path in planning** — explicit undo strategy required

**Adopted from other frameworks:**
- Complexity routing (Memory Bank, spec-then-code)
- Structured elicitation (Claude Code, BMAD)
- Spec artifacts (GitHub Spec Kit, Amazon Kiro, OpenSpec)
- Alternatives documentation (ADRs, Memory Bank /creative)
- Test-first gates (spec-then-code, Spec Kit)
- Fresh-agent review (OpenSpec, RIPER-5)
- Process reflection (Memory Bank /reflect)

## Project Structure

```
your-project/
  Docs/
    DEVELOPMENT_PROCESS.md      # The framework (copy of FRAMEWORK.md)
    archive/                    # Decision records, constraints, dead ends
      README.md                 # Archive organization rules
      process-retrospective.md  # Process reflection log
      <module-name>.md          # Per-module archive files
    design-discussions/         # Conversation logs from step 2
      YYYY-MM-DD-<slug>.md
    specs/                      # Spec artifacts from step 4
      YYYY-MM-DD-<slug>.spec.md
    templates/                  # Templates for all document types
```

## Contributing

This framework evolves through use. Process reflections (step 15) identify friction points. If you find an improvement, open an issue or PR.

## License

MIT
