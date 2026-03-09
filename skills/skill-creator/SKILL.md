---
name: skill-creator
description: Create new MAVEN skills with proper v2 SKILL.md format, frontmatter, and directory structure. Use when asked to "create a skill", "new skill", "add a capability", "build a skill", or "make a slash command".
allowed-tools:
  - Read
  - Write
  - Bash(mkdir *)
  - Glob
---

# Skill Creator

## Process

### 1. Gather Requirements
Ask the user:
- What should the skill do?
- When should it trigger? (explicit invocation, auto-detection, or both)
- What tools does it need access to?
- Should it run in a forked context or inline?
- What audience is this for?

### 2. Design the Skill
Determine:
- **name:** lowercase, hyphens, descriptive (max 64 chars)
- **description:** what it does + specific trigger phrases (max 1024 chars)
- **allowed-tools:** minimum necessary tools
- **context:** fork if heavy research/analysis, inline for quick actions
- **agent:** Explore for read-only research, general-purpose for actions
- **disable-model-invocation:** true if should only trigger via /command

### 3. Create the Skill

Directory structure:
```
.claude/skills/<skill-name>/
├── SKILL.md           # Required: instructions + frontmatter
├── references/        # Optional: supporting docs
└── scripts/           # Optional: executable code
```

### 4. Write SKILL.md
Use this template:

```yaml
---
name: <skill-name>
description: <what it does>. Use when <trigger phrases>.
allowed-tools:
  - <tool1>
  - <tool2>
---
```

Followed by markdown instructions:
- Clear step-by-step process
- Output format specification
- Guidelines and constraints
- MMA-specific context where relevant

### 5. Validate
- Verify SKILL.md is properly formatted
- Check that the skill appears in Claude's available skills
- Test with a sample invocation

## Guidelines
- Keep descriptions specific with clear trigger phrases
- Use `allowed-tools` to minimize permission prompts
- Reference files in `references/` for detailed knowledge
- Keep SKILL.md under 500 lines — move detail to references/
- Follow MMA brand voice in all skill outputs
