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

Structure depends on the task. Choose and adapt sections to fit the work.

### Examples

**Bug fix:**

```markdown
# PLAN: fix/api-timeout-on-large-export

## Problem
- `/api/export` times out when dataset exceeds 10k rows
- Users see 504 Gateway Timeout, no partial results returned

## Root Cause
- `buildExportRows()` issues one query per row (N+1)
- At 10k rows, total query time exceeds 30s gateway timeout

## Solution
- Batch lookups with `WHERE id IN (...)` — O(n) queries → O(n/500) batches
- Add streaming response so partial results are sent immediately

## Affected Files
| File | Change |
|------|--------|
| `src/export/builder.ts` | Replace per-row query with batched lookup |
| `src/routes/export.ts` | Switch to streaming response |

## Tasks
- [ ] Write failing test: export with 15k rows should complete under 10s
- [ ] Implement batched query in `buildExportRows()`
- [ ] Add streaming response handler
- [ ] Verify no regression on small datasets

## Verification
- [ ] `npm test` passes
- [ ] Manual: export 15k rows completes without timeout
- [ ] Manual: export 100 rows still works as before
```

**Feature:**

```markdown
# PLAN: feat/webhook-retry

## Goal
- Add automatic retry with exponential backoff for failed webhook deliveries
- Retain current behavior for successful deliveries (no retry, no delay)

## Design
- Max 3 retries with backoff: 1min, 5min, 30min
- Store delivery attempts in `webhook_deliveries` table
- Dead letter queue after final failure — surface in admin dashboard

## Design Decisions
- Backoff in application layer, not job queue — keeps retry policy visible and testable
- No circuit breaker for v1 — complexity not justified until we see patterns in failures

## Tasks
- [ ] Add `webhook_deliveries` migration
- [ ] Implement retry logic with backoff calculator
- [ ] Add dead letter query endpoint for admin
- [ ] Integration test: simulate 3 failures then success
- [ ] Integration test: simulate 4 failures → dead letter

## Verification
- [ ] Tests pass
- [ ] Manual: disable test endpoint, trigger webhook, observe 3 retries in logs
- [ ] Manual: confirm dead letter entry appears in admin dashboard
```

**Research / Investigation:**

```markdown
# PLAN: research/search-engine-comparison

## Question
- Should we replace Elasticsearch with OpenSearch or Meilisearch for product search?

## Scope
- Performance: indexing speed, query latency at 1M documents
- Operational: managed hosting options, backup/restore
- Out of scope: full-text search in non-product contexts

## Method
- [ ] Benchmark indexing 1M product documents on each engine
- [ ] Benchmark query latency (simple, filtered, faceted)
- [ ] Compare managed hosting costs at our projected scale
- [ ] Document trade-offs and recommendation
```

**Refactor:**

```markdown
# PLAN: refactor/extract-auth-middleware

## Current State
- Auth logic duplicated across 12 route handlers
- Each handler parses JWT, checks permissions, handles errors slightly differently

## Target State
- Single `authMiddleware(requiredPermissions)` handles all auth concerns
- Route handlers receive verified `AuthContext`, no JWT parsing

## Approach
- Extract common auth logic into middleware
- Migrate routes one-by-one, verifying each with existing tests
- No behavior changes — pure structural refactor

## Tasks
- [ ] Create `authMiddleware` with permission checking
- [ ] Migrate `/api/users` routes (highest test coverage — safest to start)
- [ ] Migrate remaining routes
- [ ] Remove duplicated auth helpers

## Verification
- [ ] All existing tests pass without modification
- [ ] No auth-related logic remains in route handlers
```

Sections can be omitted, combined, or added freely. The key principle: a plan should contain enough context that a new session can pick up the work without re-investigation.

### Verification Section

Always include a Verification section (except for pure research tasks). This defines "done":

- Concrete and actionable
- Testable by Claude during implementation
- Clear definition of "done"

This enables Claude to self-verify and iterate until the work is truly complete.

Do not start implementation until user approves the plan.
