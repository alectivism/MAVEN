---
name: end
description: End a MAVEN session — summarize the conversation, update session log and state files, optionally commit changes. Use when the user says "/end", "end session", "wrap up", "I'm done", "save and quit", or indicates they're ending their work session.
allowed-tools:
  - Read
  - Edit
  - Write
  - Bash(git *)
---

# End MAVEN Session

## Closing Sequence

### 1. Summarize the Session
Review the conversation and extract:
- **Topics covered** — what was discussed/worked on
- **Decisions made** — any choices finalized
- **Content shipped** — deliverables completed
- **Open threads** — items still in progress or needing follow-up
- **Next actions** — concrete next steps with context

### 2. Update Session Log
Append to `sessions/YYYY-MM-DD.md` (create if it doesn't exist):

```markdown
## Session: [Time or "Afternoon"/"Morning"]

### Topics
- [Topic 1]
- [Topic 2]

### Decisions
- [Decision and rationale]

### Shipped
- [Deliverable — link or description]

### Open Threads
- [Thread — status and next step]

### Next Actions
- [ ] [Action item with context]
```

**Append, don't replace** — there may be earlier sessions logged today.

### 3. Update State
Edit `state/current.md`:
- Add new priorities that emerged
- Mark completed items as done
- Update open threads
- Add any new context or deadlines
- Update the "Last Updated" timestamp

### 4. Optional: Git Commit
Ask if the user wants to commit changes:
- If yes, stage state/ and sessions/ files
- Create a commit with message: "Session log: [date] — [1-line summary]"
- Don't push unless explicitly asked

### 5. Sign Off
Brief confirmation of what was saved.
