---
name: update
description: Quick checkpoint — save current progress without ending the session. Use when the user says "/update", "checkpoint", "save progress", "quick save", or wants to preserve state mid-session.
allowed-tools:
  - Read
  - Edit
  - Write
---

# Quick Checkpoint

## Process

### 1. Assess Changes
Review conversation since last checkpoint (or session start):
- What was accomplished?
- Any new priorities or decisions?
- Any state changes worth capturing?

### 2. Update Session Log
Append a checkpoint entry to `sessions/YYYY-MM-DD.md`:

```markdown
### Checkpoint: [Time]
- [What was done since last save]
- [Key decisions or changes]
```

### 3. Update State (if material changes)
Only update `state/current.md` if there are meaningful changes:
- New priorities added
- Items completed
- Deadlines changed
- New information that affects priorities

Skip state updates for minor work — don't clutter with noise.

### 4. Confirm
Brief one-line confirmation: "Checkpoint saved. [1-sentence summary]"

## Guidelines
- This should be fast — under 30 seconds
- Don't ask questions — just save what's there
- Don't overwrite previous session log entries — append
- Only update state/ if something materially changed
