---
name: notes
description: |
  Create a NOTES file capturing discoveries and decisions from the current session.
  Suggest this when an unexpected issue was discovered and resolved, a technical decision was made
  that wasn't in the plan, the approach deviated from the original plan, significant debugging or
  investigation was performed, or the session is about to end with important context.
argument-hint: "[focus]"
---

# /pdd:notes

Create a NOTES file capturing discoveries and decisions from the current session.

## Usage

```
/pdd:notes [focus]
```

## Arguments

- `focus` (optional): Specific aspect to focus on (e.g., "debugging session", "design decision")

## Environment Variables

- `PDD_WORKSPACE_DIR`: Base directory for PLAN/NOTES (default: `.claude/pdd`)
- `PDD_LOCALE`: Output language - `en` or `ja` (default: `en`)

## Behavior

1. Get current branch name via `git branch --show-current`
2. Generate timestamp via `date +%Y%m%d_%H%M%S`
3. Create: `{PDD_WORKSPACE_DIR}/{branch}/NOTES_{timestamp}.md`

## Purpose

NOTES capture what was discovered during implementation that wasn't anticipated in PLAN:

- Unexpected issues and how they were resolved
- Technical decisions made during implementation
- Insights that should be preserved for future sessions
- Deviations from the original plan and why

## Instructions

1. Review the current conversation for significant discoveries
2. Extract key findings, decisions, and solutions
3. Write using the structured format below
4. Focus on information that would be lost when the conversation ends

## Structure

Always start with `# NOTES - {brief description}`. Choose and adapt sections based on the session's nature:

- Implementation: Accomplishments, Key Files Modified, Next Steps
- Debugging: Problem, Root Cause, Solution, Pitfalls
- Investigation/Research: Findings, Implications, Open Questions
- Design Decision: Context, Options Considered, Decision, Rationale

### Examples

**Implementation session:**

```markdown
# NOTES - Add user authentication flow

## Accomplishments
- Implemented JWT-based auth middleware
- Chose bcrypt over argon2 for password hashing (simpler dependency, sufficient for our scale)

## Key Files Modified
- `src/middleware/auth.ts` - JWT verification middleware
- `src/routes/login.ts` - login endpoint

## Next Steps
- [ ] Add refresh token rotation
- [ ] Rate limiting on login endpoint
```

**Debugging session:**

```markdown
# NOTES - Fix intermittent timeout on /api/export

## Problem
- Export endpoint times out on datasets > 10k rows

## Root Cause
- N+1 query in `buildExportRows()` — each row triggered a separate lookup

## Solution
- Batched lookups with `WHERE id IN (...)`, reduced queries from O(n) to O(1)

## Pitfalls
- Initial fix with `Promise.all` hit connection pool limit — sequential batching was needed
```

**Design decision session:**

```markdown
# NOTES - Migration strategy for auth middleware

## Context
- Current middleware stores session tokens in cookies, non-compliant with new policy

## Options Considered
1. Patch existing middleware — fast but fragile
2. Full rewrite with new token store — clean but risky during freeze
3. Adapter layer over existing code — incremental, testable

## Decision
- Option 3: adapter layer

## Rationale
- Allows incremental rollout behind feature flag during merge freeze
```

Sections can be omitted or combined freely. Keep each section concise and factual.

NOTES are meant to bridge sessions. They should enable resuming work without re-explaining context.
