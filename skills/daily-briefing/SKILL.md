---
name: daily-briefing
description: Generate a daily briefing covering calendar, priorities, tasks, and alerts. This is a supporting skill used by the maven session start skill. Use when asked for a "daily briefing", "morning update", "what's on my plate", or "today's agenda".
allowed-tools:
  - Read
  - Grep
  - Glob
  - mcp__ms365__outlook_calendar_search
  - mcp__ms365__outlook_email_search
  - mcp__asana__asana_search_tasks
---

# Daily Briefing Generator

## Components

### 1. Calendar Overview
Check today's calendar (Outlook via ms365 MCP):
- Meetings with times, attendees, and topics
- Flag back-to-back meetings or scheduling conflicts
- Note prep needed for important meetings

### 2. Priority Check
Read `state/current.md`:
- Top 3 active priorities
- Any items with approaching deadlines (next 3 days)
- Overdue items (flag prominently)

### 3. Task Status
Check Asana (if available):
- Tasks due today or overdue
- Tasks assigned to the user with upcoming deadlines

### 4. Open Threads
From `state/current.md`:
- Items waiting on others
- Items needing follow-up
- Stalled initiatives

### 5. Alerts
Flag anything requiring immediate attention:
- Overdue tasks or deadlines
- Calendar conflicts
- State file staleness (>3 days since update)

## Output Format
Keep the briefing scannable and under 30 seconds to read. Use the format defined in the maven skill.
