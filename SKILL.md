---
name: gmickel-skill
description: Guide for building Agent Skills for Claude Code, Codex, Amp, and OpenCode. Use when user asks how to create a skill, build an agent skill, write SKILL.md, distribute skills via plugins, or integrate CLI tools with AI assistants.
---

# Building Agent Skills

Agent Skills extend AI coding assistants with specialized capabilities. One SKILL.md file works across Claude Code, Codex, Amp, and OpenCode.

**Role**: Skill architecture advisor
**Goal**: Help build clean, discoverable, well-structured skills

## Official Documentation

For spec details beyond this guide, fetch from:
- https://agentskills.io/specification - YAML frontmatter, file format
- https://agentskills.io/integrate-skills - Client integration details
- https://agentskills.io/home - Overview and concepts

## When to Use This Skill

- User asks how to create/build an Agent Skill
- User wants to integrate a CLI tool with AI assistants
- User asks about SKILL.md format or structure
- User wants to distribute skills via plugins/marketplace
- User mentions making tools "AI-native"

## Quick Start

```bash
# Minimal skill structure
my-skill/
  SKILL.md           # Required - main skill file

# With references (recommended for complex skills)
my-skill/
  SKILL.md           # Main instructions (<500 lines)
  references/
    cli-reference.md # Detailed CLI docs
    examples.md      # Usage examples

# With scripts (for executable utilities)
my-skill/
  SKILL.md
  scripts/
    helper.sh        # Bash script
    process.py       # Python script
```

## Adding Scripts

Scripts provide executable utilities agents can run. Put them in `scripts/`:

```
my-skill/
  SKILL.md
  scripts/
    api.sh           # API wrapper (Bash)
    convert.py       # Processing (Python)
```

**Requirements:** Self-contained, helpful errors, handle edge cases. Supported: Python, Bash, JavaScript.

See [scripts.md](references/scripts.md) for full examples (API wrappers, processing scripts, validation).

## SKILL.md Format

```yaml
---
name: my-skill
description: >
  What this skill does and when to use it.
  Include trigger phrases the AI should recognize.
allowed-tools: Bash(mytool:*) Read  # Optional: restrict tools
---

# Skill Title

Brief intro paragraph.

## When to Use This Skill

- Trigger condition 1
- Trigger condition 2

## Quick Reference

| Command | Description |
|---------|-------------|
| `cmd one` | Does X |
| `cmd two` | Does Y |

## Common Workflows

### Workflow Name
```bash
# Step 1
mytool do-thing

# Step 2
mytool other-thing
```

## Reference Documentation

For detailed info, see:
- [cli-reference.md](references/cli-reference.md)
- [examples.md](references/examples.md)
```

## Key Principles

### 1. Progressive Disclosure

Main SKILL.md should be scannable (<500 lines). Put details in references:

```
SKILL.md          → Quick reference, common workflows
references/
  cli-reference.md  → Full CLI options
  examples.md       → Extended examples
  api-reference.md  → API details
```

### 2. Trigger-Aware Descriptions

Write descriptions that help AI know WHEN to use the skill:

**Good:**
```yaml
description: >
  Manage Raindrop.io bookmarks. Use when user mentions
  raindrop, bookmarks, saving links, organizing URLs,
  or web highlights.
```

**Bad:**
```yaml
description: A tool for managing bookmarks.
```

### 3. Safety First

For destructive operations, document safety patterns:

```markdown
## Safety Rules

1. **Never auto-send** - Create drafts first
2. **Preview before apply** - Show user what will happen
3. **Require confirmation** - Use `--confirm-send YES` pattern
```

### 4. Environment Variables

Document required env vars clearly:

```markdown
## Prerequisites

Set `MYTOOL_TOKEN` before using:
```bash
export MYTOOL_TOKEN="your_token"
```

Get token from: https://example.com/settings
```

## Installation Paths

| Client | User Scope | Project Scope |
|--------|------------|---------------|
| Claude Code | `~/.claude/skills/` | `.claude/skills/` |
| Codex | `~/.codex/skills/` | `.codex/skills/` |
| Amp | `~/.claude/skills/` | `.claude/skills/` |
| OpenCode | `~/.claude/skills/` | `.claude/skills/` |

## Distribution Patterns

### Pattern 1: Git Clone (Simple)

```bash
git clone https://github.com/user/my-skill ~/.claude/skills/my-skill
```

### Pattern 2: CLI Install Command

Add `skill install` to your CLI:

```bash
mytool skill install --target claude --scope user
mytool skill install --target codex --scope project
```

Implementation: Copy skill files to target directory atomically.

### Pattern 3: Plugin Marketplace

For skills bundled with other functionality:

```
marketplace-repo/
  .claude-plugin/
    marketplace.json
  plugins/
    my-plugin/
      .claude-plugin/
        plugin.json
      skills/
        my-skill/
          SKILL.md
      commands/
        my-command.md
```

See [plugin-structure.md](references/plugin-structure.md).

### Pattern 4: CLI --skill Flag

Output skill to stdout for piping:

```bash
mytool --skill > ~/.claude/skills/my-skill/SKILL.md
```

Good for single-file skills. For multi-file, use `skill install`.

## Reference Documentation

- [patterns.md](references/patterns.md) - Detailed pattern examples
- [plugin-structure.md](references/plugin-structure.md) - Plugin/marketplace format
- [real-examples.md](references/real-examples.md) - Production skill examples
