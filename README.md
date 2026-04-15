# MAVEN — MMA's AI Assistant

**MAVEN** = MMA's Agentic Virtual Executive Navigator

An AI assistant for MMA staff, built on [Claude Code](https://claude.ai/code). MAVEN adds persistent session management — daily briefings, goal tracking, and continuity across conversations — on top of the [MMA Plugin](https://github.com/alectivism/mma-plugin), which provides all MMA skills and context automatically.

> **Most staff don't need to install MAVEN.** The MMA Skill files are already pre-installed for all Claude users on our Team plan — you already have all MMA skills, brand guidelines, and organizational context in every Claude conversation. Install MAVEN if you want the "AI Chief of Staff" experience and help setting up Claude Code: session memory, daily briefings, and goal tracking across conversations. MAVEN requires Claude Code (the terminal app).

---

## Installation

### Before you start

- **Claude subscription required.** Claude Code requires a paid Claude plan (Team, Pro, or Max). MMA staff are on a Team plan. If you haven't been added yet, Slack Alec Foster to get set up.
- **Mac:** macOS 13 (Ventura) or later.
- **Windows:** Windows 10 or later.

---

> **STOP — Mac users must read this before doing anything else.**
>
> Claude Code will **not install** until Apple's Command Line Tools are installed on your Mac. This is a one-time setup. Do Step 1 and Step 2 before touching anything else. If you skip this and go straight to installing Claude, it will fail silently or break partway through.

---

### Step 1: Open your terminal

Your terminal is a text window where you type commands directly to your computer. You'll use it for all of the steps below.

**Mac:**
Press **Cmd+Space**, type **Terminal**, and press **Enter**. A dark or white window will appear with a blinking cursor and a line ending in `%` or `$`. That's your prompt — it means the terminal is ready.

**Windows:**
Click the **Start menu**, search for **PowerShell**, right-click it, and choose **Run as Administrator**.

> **How to paste and run commands:**
> - Mac: **Cmd+V** to paste, then **Enter** to run
> - Windows: Right-click to paste (or Ctrl+Shift+V), then **Enter** to run
>
> Run one command at a time. Wait for each one to finish before moving on.

### Step 2: Install Apple's Command Line Tools (Mac only — required)

This installs the underlying tools that Claude Code needs. **You cannot skip this step.** Paste the following command and press Enter:

```
xcode-select --install
```

**What will happen next:**

- **A popup appears** saying "The 'xcode-select' command requires the command line developer tools. Would you like to install the tools now?" — click **Install**. Do not click Get Xcode. Wait for it to finish. This takes 2–15 minutes depending on your internet speed. You'll see "The software was installed" when it's done.

- **You see: "command line tools are already installed"** — you're all set. Nothing to do. Move on to Step 3.

- **No popup appears and the terminal just returns to the prompt** — run this to verify the tools are installed:
  ```
  git --version
  ```
  If you see something like `git version 2.x.x`, you're good. If you see "command not found," contact Alec in Slack before continuing.

> Do not proceed to Step 3 until this step is complete.

### Step 3: Install Claude Code

Claude Code is the platform that powers MAVEN.

**Mac:**
```
curl -fsSL https://claude.ai/install.sh | bash
```

**Windows:**
```
irm https://claude.ai/install.ps1 | iex
```

Wait for it to finish. You'll see a success message when it's done.

**Then close your terminal window completely and open a brand new one.** This is required — the terminal needs to reload before it will recognize the `claude` command.

> **After reopening, if you type `claude` and get "command not found":**
> Paste this, press Enter, then close and reopen the terminal one more time:
> ```
> echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc && source ~/.zshrc
> ```

### Step 4: Sign in to Claude

In your new terminal window, type:
```
claude
```

A browser window will open. Sign in with your **MMA email address**. Once you see a confirmation, switch back to the terminal. You only need to do this once.

### Step 5: Download MAVEN

```
git clone https://github.com/alectivism/MAVEN.git ~/maven
```

### Step 6: Run setup

```
cd ~/maven && ./.marvin/setup.sh
```

The setup wizard walks you through:
- Your name, role, and department
- Connecting your tools (Outlook, Slack, Asana)
- Optional extras (Salesforce, AI models, etc.)

### Step 7: Start MAVEN

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
| Granola | AI meeting notes (recommended) |
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
