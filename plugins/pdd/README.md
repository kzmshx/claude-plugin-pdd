# claude-plugin-pdd

Plan-Driven Development plugin for Claude Code.

## Overview

PDD (Plan-Driven Development) is a methodology for AI-assisted development that emphasizes:

1. **PLAN.md**: Collaboratively design before implementation
2. **NOTES.md**: Capture discoveries during implementation

This plugin provides commands and automatic workflow integration.

## Features

- **Skills**: `/pdd:plan` and `/pdd:notes` - Claude can also suggest these when appropriate
- **Hooks**:
  - SessionStart: Checks for existing PLAN.md and summarizes current state
  - PreCompact: Suggests saving NOTES before context is lost

## Installation

First, add the marketplace:

```bash
/plugin marketplace add kzmshx/claude-plugins
```

Then install the plugin:

```bash
/plugin install pdd@kzmshx
```

## Skills

### /pdd:plan

Create or continue a PLAN.md for the current branch.

```bash
/pdd:plan                    # Start planning for current branch
/pdd:plan "Add user auth"    # Start with description
```

### /pdd:notes

Capture discoveries from the current session.

```bash
/pdd:notes                        # Create notes from session
/pdd:notes "debugging session"    # Create notes with focus
```

## Configuration

Set environment variables in `.claude/settings.json`:

```json
{
  "env": {
    "PDD_WORKSPACE_DIR": ".claude/pdd",
    "PDD_LOCALE": "en"
  }
}
```

| Variable            | Default       | Description                    |
| :------------------ | :------------ | :----------------------------- |
| `PDD_WORKSPACE_DIR` | `.claude/pdd` | Base directory for PLAN/NOTES  |
| `PDD_LOCALE`        | `en`          | Output language (`en` or `ja`) |

## File Structure

```txt
.claude/pdd/
└── {branch}/
    ├── PLAN.md
    ├── NOTES_20251224_143000.md
    └── NOTES_20251224_160000.md
```

## Workflow

1. Create branch for your task
2. Run `/pdd:plan` to collaboratively design the approach
3. Implement based on the plan
4. Run `/pdd:notes` when you discover something unexpected
5. Repeat steps 3-4 until complete

## Philosophy

- PLAN is a hypothesis, not a specification
- Implementation reveals unknowns that PLAN couldn't anticipate
- NOTES bridge sessions by preserving discoveries
- Human decides when to create NOTES; AI writes them
- Verification criteria in PLAN enable self-checking feedback loops

## License

MIT
