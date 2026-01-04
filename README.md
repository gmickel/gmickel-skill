# gmickel-skill

> Build Agent Skills for Claude Code, Codex, Amp, and OpenCode.

A skill that teaches AI agents how to help users build skills. Meta, but useful.

**Official docs:** [agentskills.io](https://agentskills.io)

---

## Skills I've Built

### GNO - Local Knowledge Engine

Your local second brain. Index notes, code, PDFs, Office docs. Hybrid search (BM25 + vector + reranking) and AI answers, 100% offline.

```bash
bun install -g @gmickel/gno
gno init ~/notes --name notes
gno query "auth best practices"
gno ask "summarize the API" --answer
```

**Skill install:**
```bash
gno skill install --target claude --scope user
gno skill install --target codex
```

**Also:** MCP server for Claude Desktop, Cursor, Zed, and 8 more targets.

[gno.sh](https://gno.sh) | [GitHub](https://github.com/gmickel/gno)

---

### sheets-cli - Google Sheets Primitives

Composable CLI for Google Sheets. Read tables, append rows, update by key, batch ops. JSON in, JSON out.

```bash
sheets-cli read table --sheet "Projects" --limit 10
sheets-cli update key --sheet "Projects" --key-col "Name" --key "Acme" --set '{"Status":"Done"}'
```

**Skill install:**
```bash
sheets-cli install-skill --global   # Claude Code
sheets-cli install-skill --codex    # Codex
```

[GitHub](https://github.com/gmickel/sheets-cli)

---

### raindrop-skill - Bookmark Manager

Manage Raindrop.io bookmarks, collections, tags, highlights via API.

```bash
git clone https://github.com/gmickel/raindrop-skill ~/.claude/skills/raindrop
export RAINDROP_TOKEN="your_token"
```

Uses a helper script for curl calls. Simple pattern for wrapping any REST API.

[GitHub](https://github.com/gmickel/raindrop-skill)

---

### outlookctl - Outlook Automation

Automate Classic Outlook on Windows via COM. Email and calendar with safety-first design.

- Draft-first workflow (never auto-send)
- Explicit confirmation required
- Full calendar support

```bash
uv run python tools/install_skill.py --personal
```

[GitHub](https://github.com/gmickel/outlookctl)

---

### Flow - Plan-First Workflow

Plugin with 7 skills, 5 commands, 6 agents. Plan before you code, drift never.

```bash
/plugin marketplace add https://github.com/gmickel/gmickel-claude-marketplace
/plugin install flow
/flow:plan Add OAuth login
/flow:work plans/add-oauth-login.md
```

[GitHub](https://github.com/gmickel/gmickel-claude-marketplace)

---

## Building Your Own Skills

### Quick Start

```bash
# Minimal structure
my-skill/
  SKILL.md           # Required

# With references
my-skill/
  SKILL.md           # Main (<500 lines)
  references/
    cli-reference.md
    examples.md
```

### SKILL.md Format

```yaml
---
name: my-skill
description: >
  What it does. Include trigger phrases so AI
  knows when to activate this skill.
---

# Content here
```

### What You'll Learn

| Topic | File |
|-------|------|
| SKILL.md format | [SKILL.md](SKILL.md) |
| Design patterns | [references/patterns.md](references/patterns.md) |
| Plugin structure | [references/plugin-structure.md](references/plugin-structure.md) |
| Real examples | [references/real-examples.md](references/real-examples.md) |

### Key Principles

1. **Progressive disclosure** - Main file scannable, details in references
2. **Trigger-aware descriptions** - Help AI know WHEN to use the skill
3. **Safety patterns** - Draft-first, dry-run, explicit confirmation
4. **Environment variables** - Standard pattern for auth tokens

### Distribution Options

| Method | Example | Best For |
|--------|---------|----------|
| Git clone | `git clone ... ~/.claude/skills/` | Simple skills |
| CLI install | `mytool skill install --target claude` | CLIs with skills |
| Plugin | `/plugin install flow` | Skills + commands + agents |

### Installation Paths

| Client | User | Project |
|--------|------|---------|
| Claude Code | `~/.claude/skills/` | `.claude/skills/` |
| Codex | `~/.codex/skills/` | `.codex/skills/` |
| Amp/OpenCode | `~/.claude/skills/` | `.claude/skills/` |

---

## Install This Skill

```bash
git clone https://github.com/gmickel/gmickel-skill ~/.claude/skills/gmickel-skill
```

Then ask: "How do I create an Agent Skill?"

---

## Resources

- [agentskills.io/specification](https://agentskills.io/specification) - Official spec
- [agentskills.io/integrate-skills](https://agentskills.io/integrate-skills) - Client integration

## License

MIT
