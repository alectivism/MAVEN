# MAVEN - MMA's AI Assistant

**MAVEN** = MMA's Agentic Virtual Executive Navigator

An AI assistant for Marketing + Media Alliance staff, built on [Claude Code](https://claude.ai/code). MAVEN knows MMA's org context, connects to your tools, and gets smarter as you use it.

---

## Quick Start

### 1. Install Claude Code

```bash
# Mac / Linux
curl -fsSL https://claude.ai/install.sh | bash

# Windows (PowerShell)
irm https://claude.ai/install.ps1 | iex
```

Requires a [Claude Pro or Max subscription](https://claude.ai/pricing).

### 2. Clone MAVEN

```bash
git clone https://github.com/alectivism/MAVEN.git ~/maven
cd ~/maven
```

### 3. Run Setup

```bash
./.marvin/setup.sh
```

Setup will:
- Create your profile (name, role, department)
- Set up a `maven` shell alias
- Connect your tools (MS365, Slack, Asana)
- Offer optional integrations (Salesforce, AI models, etc.)

### 4. Start Using MAVEN

```bash
maven
```

Or: `cd ~/maven && claude`

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
| `/commit` | Review and commit git changes |
| `/help` | Show all commands, skills, and integrations |
| `/sync` | Pull latest updates from GitHub |

---

## Getting Updates

```bash
cd ~/maven
git pull
```

Your personal files (CLAUDE.md, state/, sessions/, .env) are gitignored — updates never overwrite your data.

---

## How It Works

```
maven/
├── CLAUDE.md              # Your profile (yours to edit)
├── .claude/
│   ├── rules/             # MMA context & brand guidelines (updated via git pull)
│   ├── skills/            # 14 MMA-specific skills (updated via git pull)
│   └── commands/          # Slash commands (updated via git pull)
├── skills/                # Session management skills (updated via git pull)
├── state/
│   ├── current.md         # Your priorities (gitignored)
│   └── goals.md           # Your goals (gitignored)
├── sessions/              # Your daily logs (gitignored)
├── reports/               # Your weekly reports (gitignored)
└── .env                   # Your API keys (gitignored)
```

**Tracked files** (`.claude/rules/`, `.claude/skills/`, `.claude/commands/`, `skills/`) get updated when you `git pull`. These contain MMA org context, brand guidelines, and skill definitions.

**Your files** (`CLAUDE.md`, `state/`, `sessions/`, `.env`) are gitignored and never touched by updates.

---

## Integrations

### Core (offered during setup)
| Integration | What It Does |
|-------------|--------------|
| Microsoft 365 | Outlook, Calendar, OneDrive, SharePoint |
| Parallel Search | Web search from MAVEN |
| Slack | Team messaging and search |
| Asana | Task and project management |

### Optional (offered during setup)
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
- Ask MAVEN anything: "What can you do?" or "How do I connect Salesforce?"
- Reach out on **#ai-upskilling** in Slack

---

*Built on [MARVIN template](https://github.com/SterlingChin/marvin-template) by Sterling Chin. MMA customization by Alec Foster.*
