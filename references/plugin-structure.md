# Plugin & Marketplace Structure

Distribute skills as part of a Claude Code plugin marketplace.

## When to Use Plugins

- Skill + commands + agents together
- Multiple related skills
- Version management needed
- Marketplace distribution

For simple standalone skills, git clone is usually enough.

## Directory Structure

```
my-marketplace/
  .claude-plugin/
    marketplace.json        # Marketplace manifest
  plugins/
    my-plugin/
      .claude-plugin/
        plugin.json         # Plugin manifest
      skills/
        my-skill/
          SKILL.md
          references/
            ...
      commands/
        my-command.md       # Slash command
      agents/
        my-agent.md         # Subagent definition
      README.md
  README.md
  LICENSE
```

## Marketplace Manifest

`.claude-plugin/marketplace.json`:

```json
{
  "name": "my-marketplace",
  "owner": {
    "name": "Your Name",
    "url": "https://github.com/username"
  },
  "metadata": {
    "description": "Description of marketplace",
    "version": "0.1.0"
  },
  "plugins": [
    {
      "name": "my-plugin",
      "description": "What the plugin does",
      "version": "1.0.0",
      "author": {
        "name": "Your Name",
        "email": "you@example.com",
        "url": "https://example.com"
      },
      "homepage": "https://example.com/my-plugin",
      "tags": ["keyword1", "keyword2"],
      "source": "./plugins/my-plugin"
    }
  ]
}
```

## Plugin Manifest

`plugins/my-plugin/.claude-plugin/plugin.json`:

```json
{
  "name": "my-plugin",
  "version": "1.0.0",
  "description": "What the plugin does",
  "author": {
    "name": "Your Name",
    "email": "you@example.com",
    "url": "https://example.com"
  },
  "homepage": "https://example.com/my-plugin",
  "repository": "https://github.com/username/my-marketplace",
  "license": "MIT",
  "keywords": ["keyword1", "keyword2"]
}
```

## Version Sync

Keep versions in sync between:
- `.claude-plugin/marketplace.json` (plugin entry)
- `plugins/my-plugin/.claude-plugin/plugin.json`

Bump version script:

```bash
#!/usr/bin/env bash
# scripts/bump.sh <patch|minor|major> <plugin-name>
BUMP_TYPE=$1
PLUGIN=$2

# Update both manifests
# ... version bump logic
```

## Slash Commands

Commands live in `plugins/my-plugin/commands/`:

```markdown
# my-command.md

Instruction prompt for the command.
This becomes `/my-plugin:my-command`.
```

Commands can invoke skills:

```markdown
# analyze.md

Use the my-skill skill to analyze the user's request.
Follow the workflow in SKILL.md.
```

## Subagents

Agents live in `plugins/my-plugin/agents/`:

```markdown
# my-agent.md

---
name: my-agent
description: When to spawn this agent
tools: Read, Grep, Glob, Bash
---

Agent instructions here.
```

## Skills in Plugins

Skills in plugins follow the same SKILL.md format:

```
plugins/my-plugin/skills/my-skill/
  SKILL.md
  references/
    cli-reference.md
```

## Installation Flow

Users install via Claude Code:

```
/plugin marketplace add https://github.com/username/my-marketplace
/plugin install my-plugin
```

## Stub Commands Pattern

Use commands as entry points to skills:

```markdown
# Command: /my-plugin:action

This is a stub command. Invoke the my-skill skill with:

#$ARGUMENTS

The skill handles all logic. This command just routes.
```

Benefits:
- Users discover via `/` commands
- Skills handle implementation
- Clean separation of concerns
