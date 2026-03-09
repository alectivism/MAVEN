---
name: commit
description: Review git changes and create clean, well-structured commits. Use when the user says "/commit", "commit changes", "save to git", or wants to commit their work.
allowed-tools:
  - Bash(git *)
  - Read
---

# Git Commit Workflow

## Process

### 1. Review Changes
Run `git status` and `git diff` to see:
- Staged changes
- Unstaged modifications
- Untracked files

### 2. Group Changes
Identify logical groups:
- **feat:** New capabilities or features
- **fix:** Bug fixes
- **docs:** Documentation changes
- **chore:** Maintenance, config changes
- **refactor:** Code restructuring
- **content:** New content files
- **session:** Session logs and state updates

### 3. Stage & Commit
For each logical group:
- Stage the relevant files (`git add` specific files, not `git add .`)
- Write a clear commit message:
  - First line: `type: brief description` (under 72 chars)
  - Blank line
  - Body: context if needed

### 4. Confirm
Show the user what will be committed before executing.
Never push unless explicitly asked.

## Guidelines
- Never commit `.env` or credentials files
- Don't commit large binary files without asking
- Prefer multiple small commits over one large commit
- Session logs and state files can be grouped as one commit
