# gmickel-skill

> Learn to build Agent Skills for Claude Code, Codex, Amp, and OpenCode.

This skill teaches you how to create skills. Meta, but useful.

## What Are Agent Skills?

Agent Skills are markdown files that extend AI coding assistants with specialized capabilities. Write once, works everywhere.

**Official docs:** [agentskills.io](https://agentskills.io)

## Quick Start

```bash
# Install as skill (meta!)
git clone https://github.com/gmickel/gmickel-skill ~/.claude/skills/gmickel-skill

# Or just read the docs
cat SKILL.md
```

Then ask your AI assistant:
- "How do I create an Agent Skill?"
- "What's the SKILL.md format?"
- "How do I distribute skills via plugins?"

## What You'll Learn

| Topic | File |
|-------|------|
| SKILL.md format | [SKILL.md](SKILL.md) |
| Design patterns | [references/patterns.md](references/patterns.md) |
| Plugin structure | [references/plugin-structure.md](references/plugin-structure.md) |
| Real examples | [references/real-examples.md](references/real-examples.md) |

## Key Concepts

### 1. SKILL.md Format

```yaml
---
name: my-skill
description: What it does. Include trigger phrases.
---

# Content here
```

### 2. Progressive Disclosure

Keep SKILL.md scannable (<500 lines). Details go in `references/`.

### 3. Distribution Options

- **Git clone** - Simple, works everywhere
- **CLI install** - `mytool skill install --target claude`
- **Plugin marketplace** - Bundle with commands and agents

## Real Skills to Study

| Skill | Pattern | Repo |
|-------|---------|------|
| raindrop-skill | API wrapper + helper script | [github](https://github.com/gmickel/raindrop-skill) |
| sheets-cli | CLI with install-skill | [github](https://github.com/gmickel/sheets-cli) |
| outlookctl | Safety-first, draft workflow | [github](https://github.com/gmickel/outlookctl) |
| gno | Full skill management CLI | [github](https://github.com/gmickel/gno) |
| flow | Plugin with multiple skills | [github](https://github.com/gmickel/gmickel-claude-marketplace) |

## Files

```
gmickel-skill/
  SKILL.md                          # Main skill instructions
  references/
    patterns.md                     # YAML, safety, env vars
    plugin-structure.md             # Marketplace distribution
    real-examples.md                # Production examples
```

## Related Resources

- [Agent Skills Spec](https://agentskills.io/specification) - Official format
- [Integration Guide](https://agentskills.io/integrate-skills) - Client integration
- [gmickel-claude-marketplace](https://github.com/gmickel/gmickel-claude-marketplace) - Plugin example

## License

MIT
