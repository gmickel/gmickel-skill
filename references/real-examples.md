# Real-World Skill Examples

Production skills demonstrating best practices.

## 1. API Wrapper Skill (raindrop-skill)

Wraps a REST API for bookmark management.

**Structure:**
```
raindrop-skill/
  SKILL.md
  scripts/
    raindrop.sh         # API helper script
  references/
    API-REFERENCE.md    # Full API docs
```

**Key patterns:**
- Helper script for curl calls
- Environment variable for auth (`RAINDROP_TOKEN`)
- Quick reference tables
- Full API coverage in references

**SKILL.md excerpt:**
```yaml
---
name: raindrop
description: >
  Manage Raindrop.io bookmarks, collections, tags, highlights.
  Use when user mentions raindrop, bookmarks, saving links,
  organizing URLs, or web highlights.
---

# Raindrop.io

## Quick Reference

| Operation | Command |
|-----------|---------|
| List collections | `raindrop.sh GET /collections` |
| Search bookmarks | `raindrop.sh GET '/raindrops/0?search=query'` |
| Save link | `raindrop.sh POST /raindrop '{"link":"url"}'` |
```

**Repo:** github.com/gmickel/raindrop-skill

---

## 2. CLI Tool Skill (sheets-cli)

Skill for a full CLI tool with multiple commands.

**Structure:**
```
sheets-cli/
  SKILL.md              # Embedded in repo
  src/                  # CLI source code
```

**Key patterns:**
- CLI has `install-skill` command
- Skill documents CLI interface
- Best practices section
- Exit codes documented

**Install pattern:**
```bash
sheets-cli install-skill --global   # ~/.claude/skills/
sheets-cli install-skill --codex    # ~/.codex/skills/
```

**SKILL.md excerpt:**
```yaml
---
name: sheets-cli
description: >
  Read, write, update Google Sheets via CLI. Use when user
  asks to read spreadsheet data, update cells, append rows,
  or work with Google Sheets.
---

## Workflow Pattern

Always: **read -> decide -> dry-run -> apply**

```bash
# 1. Understand current state
sheets-cli read table --sheet "Tasks" --limit 100

# 2. Dry-run first
sheets-cli update key --sheet "Tasks" --key-col "ID" \
  --key "TASK-42" --set '{"Status":"Complete"}' --dry-run

# 3. Apply if correct
sheets-cli update key ...
```
```

**Repo:** github.com/gmickel/sheets-cli

---

## 3. COM Automation Skill (outlookctl)

Windows-specific skill for Outlook automation.

**Structure:**
```
outlookctl/
  skills/
    outlook-automation/
      SKILL.md
      reference/
        cli.md
        json-schema.md
        security.md
        troubleshooting.md
```

**Key patterns:**
- Safety-first (draft-first workflow)
- Explicit confirmation required
- Platform-specific requirements
- Detailed reference docs

**SKILL.md excerpt:**
```yaml
---
name: outlook-automation
description: >
  Automates Classic Outlook on Windows via COM. Use for
  email and calendar. Requires Classic Outlook running.
---

## Safety Rules

**CRITICAL: Follow these rules:**

1. **Never auto-send** - Always create drafts first
2. **Draft-first workflow** - `draft` -> preview -> `send`
3. **Explicit confirmation** - Requires `--confirm-send YES`
```

**Repo:** github.com/gmickel/outlookctl

---

## 4. Local Search Skill (gno)

Full-featured CLI with skill management built-in.

**Structure:**
```
gno/
  assets/
    skill/
      SKILL.md
      cli-reference.md
      examples.md
      mcp-reference.md
  src/
    cli/
      commands/
        skill/
          install.ts
          uninstall.ts
          show.ts
          paths.ts
```

**Key patterns:**
- `gno skill install` command
- `gno skill show` previews without installing
- `gno skill paths` shows install locations
- Multi-target support (claude, codex)

**CLI install:**
```bash
gno skill install --target claude --scope user
gno skill install --target codex --scope project
```

**SKILL.md excerpt:**
```yaml
---
name: gno
description: >
  Search local documents and knowledge bases. Index directories,
  search with BM25/vector/hybrid, get AI answers with citations.
  Use when user wants to search files, find documents, query notes.
allowed-tools: Bash(gno:*) Read
---

## Core Commands

| Command | Description |
|---------|-------------|
| `gno search <q>` | BM25 keyword search |
| `gno vsearch <q>` | Vector semantic search |
| `gno query <q>` | Hybrid (BM25 + vector + rerank) |
| `gno ask <q>` | AI answer with citations |
```

**Repo:** github.com/gmickel/gno

---

## 5. Plugin with Skills (flow)

Multiple skills bundled in a plugin.

**Structure:**
```
gmickel-claude-marketplace/
  plugins/
    flow/
      .claude-plugin/
        plugin.json
      skills/
        flow-plan/
          SKILL.md
          steps.md
          examples.md
        flow-work/
          SKILL.md
          phases.md
        interview/
          SKILL.md
      commands/
        flow/
          plan.md
          work.md
      agents/
        repo-scout.md
        practice-scout.md
```

**Key patterns:**
- Commands invoke skills
- Skills reference step files
- Agents for parallel research
- Version sync between manifests

**Command -> Skill pattern:**
```markdown
# commands/flow/plan.md

Invoke the flow-plan skill to create a plan.
Input: #$ARGUMENTS
```

**Repo:** github.com/gmickel/gmickel-claude-marketplace

---

## Common Patterns Summary

| Pattern | Example | When to Use |
|---------|---------|-------------|
| Helper script | raindrop.sh | API without CLI |
| CLI install-skill | sheets-cli, gno | CLI tool with skill |
| Draft-first | outlookctl | Destructive operations |
| Plugin bundle | flow | Multiple skills + commands |
| Progressive refs | all | Complex documentation |
