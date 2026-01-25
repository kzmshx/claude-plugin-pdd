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

Use this format for consistency and searchability:

```markdown
# NOTES - {brief description}

## Accomplishments
- What was completed and why (include decision rationale)

## Lessons Learned
- What worked well
- What didn't work (failed approaches to avoid)
- Unexpected behaviors discovered

## Next Steps
- [ ] Concrete next actions
- [ ] Open tasks remaining

## Key Files Modified
- `path/to/file.ts` - brief description of change

## Open Questions
- Unresolved issues or decisions needed
```

Sections can be omitted if not applicable. Keep each section concise and factual.

NOTES are meant to bridge sessions. They should enable resuming work without re-explaining context.
