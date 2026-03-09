# MAVEN — MMA's AI Assistant

**MAVEN** = MMA's Agentic Virtual Executive Navigator

An AI assistant for Marketing + Media Alliance staff, built on [Claude Code](https://claude.ai/code). MAVEN knows MMA's org context, connects to your everyday tools (Outlook, Slack, Asana), and remembers your work across sessions.

---

## Installation

### Step 1: Make sure you have Claude Code

Claude Code is the platform that powers MAVEN. If you haven't installed it yet:

**Mac:**
1. Open **Terminal** (search "Terminal" in Spotlight, or find it in Applications > Utilities)
2. Paste this command and press Enter:
```
curl -fsSL https://claude.ai/install.sh | bash
```
3. Close and reopen Terminal

**Windows:**
1. Open **PowerShell** (search "PowerShell" in the Start menu)
2. Paste this command and press Enter:
```
irm https://claude.ai/install.ps1 | iex
```
3. Close and reopen PowerShell

Claude Code requires a Claude subscription (Team, Pro, or Max). MMA staff have a Team subscription — use your MMA email to sign in when prompted.

### Step 2: Download MAVEN

Paste this into your terminal and press Enter:
```
git clone https://github.com/alectivism/MAVEN.git ~/maven
```

> **First time using the terminal on Mac?** You may see a popup asking to install "Command Line Developer Tools." Click **Install** and wait for it to finish (a few minutes), then run the command above again.

### Step 3: Run Setup

```
cd ~/maven && ./.marvin/setup.sh
```

The setup wizard walks you through:
- Your name, role, and department
- Connecting your tools (Outlook, Slack, Asana)
- Optional extras (Salesforce, AI models, etc.)

### Step 4: Start MAVEN

```
maven
```

That's it. Type `/maven` inside to get your first daily briefing.

---

## Daily Usage

**Start your day:**
```
maven
```
Then type `/maven` for a briefing of your priorities, calendar, and tasks.

**Work naturally** — just talk:
- "Draft an email to the membership team about Q2 renewals"
- "What's on my calendar today?"
- "Create an Asana task for the board deck, due Friday"
- "Summarize what happened in #general yesterday"
- "Prep me for my 2pm call with Hassan"

**Save your progress:** Type `/update`

**End your day:** Type `/end`

MAVEN does not auto-save. Use `/update` periodically and `/end` when you're done.

---

## What MAVEN Can Do

### Writing & Content
- **Email drafting** — audience-aware (internal, members, board, external) with MMA formatting
- **Content creation** — blog posts, newsletters, thought leadership with MMA data points
- **Social posts** — LinkedIn-optimized with engagement templates
- **Event promotion** — POSSIBLE, CMO Summit, SMARTIES, CATS, AIPOSSIBLE
- **Member communications** — segment-aware onboarding, renewals, announcements
- **Case studies** — from lab results and member outcomes

### Research & Analysis
- **Web research** — with MMA context and lens
- **Lab summaries** — Future Lab results in executive, member, marketing, or detailed format
- **Research briefs** — think tank findings (MATT, MOSTT, ALTT, DATT) for any audience
- **Meeting prep** — attendee info, past interactions, talking points

### Operations
- **Meeting follow-ups** — segmented post-meeting emails with proper attribution
- **Asana tasks** — create, update, manage with MMA naming conventions
- **Slack summaries** — channel activity digests with key decisions and action items
- **SharePoint navigation** — find documents across 17+ MMA sites

### Memory & Continuity
- **Session management** — pick up where you left off
- **Goal tracking** — monitor progress across sessions
- **Daily briefings** — priorities, deadlines, calendar, alerts

---

## Commands

| Command | What It Does |
|---------|--------------|
| `/maven` | Start your day with a briefing |
| `/end` | End session and save everything |
| `/update` | Quick checkpoint (save progress) |
| `/report` | Generate a weekly summary |
| `/help` | Show all commands, skills, and integrations |

---

## Getting Updates

When new skills or MMA context updates are available:

```
cd ~/maven
git pull
```

Your personal files (profile, goals, session logs) are never overwritten — only shared skills and MMA context update.

---

## Integrations

### Core (set up during install)
| Integration | What It Does |
|-------------|--------------|
| Microsoft 365 | Outlook, Calendar, OneDrive, SharePoint |
| Web Search | Search the web from MAVEN |
| Slack | Team messaging and search |
| Asana | Task and project management |

### Optional (offered during install)
| Integration | What It Does |
|-------------|--------------|
| Fireflies | Meeting transcription |
| Salesforce | CRM, membership, pipeline |
| OpenAI | GPT models |
| Gemini | Google AI models |
| ElevenLabs | Text-to-speech, audio |
| Context7 | Library documentation lookup |
| Exa | AI-powered web search |

---

## Need Help?

- Type `/help` inside MAVEN
- Ask MAVEN: "What can you do?" or "How do I connect Salesforce?"
- Reach out on **#ai-upskilling** in Slack

---

*Built on [MARVIN template](https://github.com/SterlingChin/marvin-template) by Sterling Chin. MMA customization by Alec Foster.*
