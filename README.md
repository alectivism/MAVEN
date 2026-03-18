# MAVEN — MMA's AI Assistant

**MAVEN** = MMA's Agentic Virtual Executive Navigator

An AI assistant for MMA staff, built on [Claude Code](https://claude.ai/code). MAVEN adds persistent session management — daily briefings, goal tracking, and continuity across conversations — on top of the [MMA Plugin](https://github.com/alectivism/mma-plugin), which provides all MMA skills and context automatically.

> **Most staff don't need to install MAVEN.** The MMA Skill files are already pre-installed for all Claude users on our Team plan — you already have all MMA skills, brand guidelines, and organizational context in every Claude conversation. Install MAVEN if you want the "AI Chief of Staff" experience and help setting up Claude Code: session memory, daily briefings, and goal tracking across conversations. MAVEN requires Claude Code (the terminal app).

---

## Installation

### Step 1: Make sure you have Claude Code

Claude Code is the platform that powers MAVEN. If you haven't installed it yet:

**Mac:**
1. Open **Terminal** (press Cmd+Space, type "Terminal", press Enter)
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

Claude Code requires a Claude subscription (Team, Pro, or Max). MMA staff are on a Team plan — sign in with your MMA email when prompted. If you haven't been added to the Team plan yet, Slack Alec Foster to get set up.

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

### Starting MAVEN

Every time you want to use MAVEN, open your terminal and type:
```
maven
```

This opens Claude Code in your MAVEN folder. You'll see a text prompt where you can type messages to MAVEN.

> **What's the difference between `maven` and `/maven`?**
> - `maven` is what you type in your **terminal** to launch Claude Code
> - `/maven` is what you type **inside Claude Code** to get your daily briefing
>
> Think of it like: `maven` opens the app, `/maven` starts your workday.

### Your typical day

1. **Open your terminal** and type `maven` to launch MAVEN
2. **Type `/maven`** to get a briefing of your priorities, calendar, and tasks
3. **Work naturally** — just type what you need in plain English:
   - "Draft an email to the membership team about Q2 renewals"
   - "What's on my calendar today?"
   - "Create an Asana task for the board deck, due Friday"
   - "Summarize what happened in #general yesterday"
   - "Prep me for my 2pm call with Hassan"
4. **Type `/update`** periodically to save your progress
5. **Type `/end`** when you're done for the day

MAVEN does not auto-save. If you close the terminal without running `/end`, your session won't be saved for next time.

---

## What MAVEN Adds (Beyond the MMA Plugin)

The MMA Plugin is pre-installed for all staff on the Claude Team plan — you already have 17 MMA skills, brand guidelines, and org context in every conversation. MAVEN adds the "Chief of Staff" layer on top:

### Session Management & Memory
- **Daily briefings** (`/maven`) — priorities, deadlines, calendar, open threads
- **Session continuity** — pick up where you left off, even days later
- **Goal tracking** — monitor progress across sessions
- **Checkpoints** (`/update`) — save progress mid-session
- **Session logs** (`/end`) — automatic daily summaries

### Tool Integrations
MAVEN connects to your everyday tools during setup: Outlook, Slack, Asana, SharePoint, and optional extras (Salesforce, Fireflies, AI models). These connections are per-user and require individual authentication.

---

## Commands

Type these **inside MAVEN** (inside Claude Code, not in the regular terminal):

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

1. Open your terminal
2. Run these two commands:
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
- Reach out on **#ai-af-agents** in Slack

---

*Inspired by [MARVIN template](https://github.com/SterlingChin/marvin-template) by Sterling Chin. MMA customization + custom skills by Alec Foster.*
