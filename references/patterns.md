# Skill Patterns

Detailed patterns for building effective Agent Skills.

## YAML Frontmatter

### Required Fields

```yaml
---
name: skill-name        # Lowercase, hyphenated
description: >          # Multi-line description
  What the skill does.
  Include trigger phrases.
---
```

### Optional Fields

```yaml
---
name: skill-name
description: Brief description
allowed-tools: Bash(mytool:*) Read Write  # Restrict available tools
---
```

Tool restrictions follow Claude Code syntax:
- `Bash(prefix:*)` - Allow bash commands starting with prefix
- `Read` - Allow file reading
- `Write` - Allow file writing
- `Edit` - Allow file editing
- `Glob` - Allow file pattern matching
- `Grep` - Allow content searching

## Description Best Practices

### Trigger Phrases

Include phrases users might say:

```yaml
description: >
  Search local documents and knowledge bases. Use when user
  wants to search files, find documents, query notes, look up
  information, index a directory, set up document search, or
  needs semantic/vector search.
```

### Negative Triggers

Clarify when NOT to use:

```yaml
description: >
  Automate Classic Outlook on Windows via COM. Use for email
  and calendar operations. NOT for Outlook web or Exchange APIs.
```

## Progressive Disclosure Structure

### Main SKILL.md (~200-400 lines)

```markdown
# Skill Name

One-paragraph intro.

## Quick Reference

| Command | Description |
|---------|-------------|
| `cmd a` | Does A |
| `cmd b` | Does B |

## Common Workflows

### Workflow 1
Steps...

### Workflow 2
Steps...

## Reference

See [cli-reference.md](references/cli-reference.md) for all options.
```

### Reference Files

```
references/
  cli-reference.md      # Full CLI documentation
  api-reference.md      # API endpoints (if applicable)
  examples.md           # Extended examples
  troubleshooting.md    # Common issues
  security.md           # Security considerations
```

## Table Formatting

Quick reference tables are highly effective:

```markdown
| Command | Description |
|---------|-------------|
| `search <query>` | BM25 keyword search |
| `query <query>` | Hybrid search with rerank |
| `ask <question>` | AI answer with citations |
```

For complex commands, use nested tables:

```markdown
### Command Options

| Option | Default | Description |
|--------|---------|-------------|
| `-n` | 5 | Max results |
| `--json` | false | JSON output |
```

## Code Examples

### Single Command

```markdown
```bash
mytool search "query" --limit 10
```
```

### Multi-Step Workflow

```markdown
```bash
# 1. Check current state
mytool status

# 2. Preview changes
mytool apply --dry-run

# 3. Apply if correct
mytool apply
```
```

### With Output

```markdown
```bash
$ mytool status
Collection: docs (42 files)
Last indexed: 2024-01-15
```
```

## Safety Patterns

### Draft-First Workflow

```markdown
## Safety Rules

1. **Never auto-send** - Always create drafts first
2. **Show preview** - Display what will be sent
3. **Require confirmation** - Only send after explicit approval

### Send Workflow

```bash
# 1. Create draft
mytool draft --to "user@example.com" --subject "Hi"

# 2. Show preview to user (your job as AI)

# 3. Send only after user confirms
mytool send --draft-id "123" --confirm-send YES
```
```

### Destructive Operations

```markdown
## Caution

`delete` operations are permanent. Always:
1. Show what will be deleted
2. Ask for confirmation
3. Use `--dry-run` when available
```

## Environment Variables

### Documentation Pattern

```markdown
## Prerequisites

Set `MYTOOL_TOKEN` in your shell:

```bash
# Add to ~/.zshrc.local (not committed to dotfiles)
export MYTOOL_TOKEN="your_token"
```

Get token from: https://example.com/settings/tokens

### Why Environment Variables?

- Standard pattern (like GH_TOKEN, ANTHROPIC_API_KEY)
- Inherited by subprocesses and agents
- Secure when stored in .zshrc.local
```

### Verification Pattern

```markdown
### Verify Setup

```bash
curl -s "https://api.example.com/user" \
  -H "Authorization: Bearer $MYTOOL_TOKEN" | jq .name
```
```

## Output Formats

### JSON Output

Document JSON structure for AI parsing:

```markdown
## Output Format

All commands return JSON:

```json
{
  "ok": true,
  "result": { ... }
}
```

Errors:
```json
{
  "ok": false,
  "error": { "code": "AUTH_ERROR", "message": "..." }
}
```
```

### Exit Codes

```markdown
## Exit Codes

| Code | Meaning |
|------|---------|
| 0 | Success |
| 1 | Validation error |
| 2 | Auth error |
| 3 | API error |
```

## Helper Scripts

For API-based skills without a CLI:

```markdown
## Helper Script

Use `scripts/mytool.sh` for API calls:

```bash
./scripts/mytool.sh GET /endpoint
./scripts/mytool.sh POST /endpoint '{"key":"value"}'
```

The script handles authentication via `$MYTOOL_TOKEN`.
```

Example script:

```bash
#!/usr/bin/env bash
set -euo pipefail

BASE_URL="https://api.example.com/v1"

if [[ -z "${MYTOOL_TOKEN:-}" ]]; then
    echo "Error: MYTOOL_TOKEN not set" >&2
    exit 1
fi

METHOD="$1"
ENDPOINT="$2"
BODY="${3:-}"

ARGS=(-s -X "$METHOD" "${BASE_URL}${ENDPOINT}"
      -H "Authorization: Bearer $MYTOOL_TOKEN"
      -H "Content-Type: application/json")

[[ -n "$BODY" ]] && ARGS+=(-d "$BODY")

curl "${ARGS[@]}" | jq .
```
