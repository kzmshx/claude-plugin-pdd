---
name: plan
description: |
  Create or continue PLAN.md for the current branch.
  Use when starting a new feature, bug fix, refactoring, or any task that benefits from upfront planning.
  Suggest this when user is on a feature branch (not main/master) without an existing PLAN.md.
argument-hint: "[description]"
---

# /pdd:plan

Create or continue a PLAN.md for the current branch.

## Usage

```
/pdd:plan [description]
```

## Environment Variables

- `PDD_WORKSPACE_DIR`: Base directory for PLAN/NOTES (default: `.claude/pdd`)
- `PDD_LOCALE`: Output language - `en` or `ja` (default: `en`)

## Behavior

1. Get current branch name via `git branch --show-current`
2. Resolve path: `{PDD_WORKSPACE_DIR}/{branch}/PLAN.md`
3. If PLAN.md exists, read and continue editing
4. If not, create through discussion with user

## Instructions

PLAN.md is created collaboratively through discussion, not by filling a fixed template.

1. Ask what the user wants to accomplish
2. Clarify goals, constraints, and approach through conversation
3. Write PLAN.md reflecting the discussion
4. Review with user before starting implementation

Structure depends on the task. Adapt sections to fit the work:

- Feature: Overview, Background, Goal, Design, Tasks, Verification
- Bug fix: Problem, Cause, Solution, Tasks, Verification
- Research: Question, Scope, Method
- Refactor: Current State, Target State, Approach, Verification

## Verification Section

Always include a Verification section (except for pure research tasks). This defines "done":

```markdown
## Verification
- [ ] Tests pass (`npm test` / `pytest` / etc.)
- [ ] Manual verification: [specific steps]
- [ ] No regressions in existing functionality
```

The verification criteria should be:

- Concrete and actionable
- Testable by Claude during implementation
- Clear definition of "done"

This enables Claude to self-verify and iterate until the work is truly complete.

Do not start implementation until user approves the plan.
