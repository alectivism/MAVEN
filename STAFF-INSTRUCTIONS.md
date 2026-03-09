# Getting Started with MMA MAVEN

**MAVEN** = MMA's Agentic Virtual Executive Navigator

Your AI assistant that knows MMA, connects to your tools, and remembers your work across sessions.

---

## What You Need

- **A computer** (Mac, Windows, or Linux)
- **A Claude Pro or Max subscription** — [claude.ai/pricing](https://claude.ai/pricing)
- **Your MMA credentials** for Outlook, Slack, and Asana

---

## Setup (15 minutes, one time)

### Step 1: Install Claude Code

**Mac / Linux:**
```
curl -fsSL https://claude.ai/install.sh | bash
```

**Windows (PowerShell):**
```
irm https://claude.ai/install.ps1 | iex
```

Close and reopen your terminal, then verify:
```
claude --version
```

### Step 2: Install Git (if you don't have it)

Check if you already have it:
```
git --version
```

If not installed:
- **Mac:** Install Xcode Command Line Tools: `xcode-select --install`
- **Windows:** Download from [git-scm.com](https://git-scm.com)

### Step 3: Clone MAVEN

```
git clone https://github.com/alectivism/MAVEN.git ~/maven
cd ~/maven
```

### Step 4: Run Setup

```
./.marvin/setup.sh
```

The setup wizard will walk you through:
1. Your name, role, and department
2. Communication style preferences
3. Tool connections (MS365, Slack, Asana)
4. Optional integrations

### Step 5: Start MAVEN

```
maven
```

Type `/maven` to get your first briefing.

---

## Daily Usage

### Start Your Day
```
maven
```
Then type `/maven` for a briefing of your priorities, calendar, and tasks.

### Work Naturally
Just talk to MAVEN:
- "Draft an email to the membership team about Q2 renewals"
- "What's on my calendar today?"
- "Create an Asana task for the board deck, due Friday"
- "Summarize what happened in #general yesterday"
- "Prep me for my 2pm call with Hassan"

### Save Your Progress
```
/update
```

### End Your Day
```
/end
```

MAVEN does not auto-save. Use `/update` periodically and `/end` when done.

---

## Getting Updates

When the team pushes new skills or updated MMA context:

```
cd ~/maven
git pull
```

Your personal files are never overwritten — only shared skills and MMA context update.

---

## Commands

| Command | What It Does |
|---------|--------------|
| `/maven` | Start with a briefing |
| `/end` | Save and end session |
| `/update` | Quick save |
| `/report` | Weekly summary |
| `/help` | Show all commands and skills |

---

## Troubleshooting

**"claude: command not found"**
Close and reopen your terminal after installing Claude Code.

**"maven: command not found"**
Re-run setup: `./.marvin/setup.sh` — it creates the shell alias.

**Can't connect to MS365/Slack/Asana**
Ask MAVEN: "Help me set up Microsoft 365" — it will walk you through the MCP setup.

**Git pull shows conflicts**
Your personal files are gitignored, so this shouldn't happen. If it does, ask in #ai-upskilling.

---

## Need Help?

- Type `/help` inside MAVEN
- Ask on **#ai-upskilling** in Slack
- Contact Alec Foster for technical issues
