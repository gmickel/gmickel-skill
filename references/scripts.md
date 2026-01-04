# Adding Scripts to Skills

Scripts provide executable utilities that agents can run. Put them in a `scripts/` directory.

## Directory Structure

```
my-skill/
  SKILL.md
  scripts/
    api.sh           # API wrapper
    convert.py       # Processing utility
    validate.js      # Validation script
```

## Requirements

From the official spec:
- **Self-contained** or clearly document dependencies
- **Include helpful error messages**
- **Handle edge cases gracefully**
- **Supported languages**: Python, Bash, JavaScript

## Referencing Scripts from SKILL.md

```markdown
## Helper Script

Use the API helper for requests:

```bash
./scripts/api.sh GET /endpoint
./scripts/api.sh POST /endpoint '{"key":"value"}'
```
```

## Example: API Wrapper (Bash)

Pattern used by raindrop-skill for REST API calls:

```bash
#!/usr/bin/env bash
set -euo pipefail

BASE_URL="https://api.example.com/v1"

if [[ -z "${API_TOKEN:-}" ]]; then
    echo "Error: API_TOKEN not set" >&2
    exit 1
fi

METHOD="$1"
ENDPOINT="$2"
BODY="${3:-}"

ARGS=(-s -X "$METHOD" "${BASE_URL}${ENDPOINT}"
      -H "Authorization: Bearer $API_TOKEN"
      -H "Content-Type: application/json")

[[ -n "$BODY" ]] && ARGS+=(-d "$BODY")

curl "${ARGS[@]}" | jq .
```

**Usage:**
```bash
./scripts/api.sh GET /users
./scripts/api.sh POST /items '{"name":"test"}'
./scripts/api.sh DELETE /items/123
```

## Example: Processing Script (Python)

Pattern from anthropic/skills/pdf for file processing:

```python
#!/usr/bin/env python3
"""Convert input files to output format."""
import sys
import os

def process(input_path, output_dir):
    """Core processing logic."""
    if not os.path.exists(input_path):
        print(f"Error: {input_path} not found", file=sys.stderr)
        sys.exit(1)

    os.makedirs(output_dir, exist_ok=True)

    # Processing logic here
    output_file = os.path.join(output_dir, "output.txt")
    print(f"Processing {input_path} -> {output_file}")

if __name__ == "__main__":
    if len(sys.argv) != 3:
        print("Usage: process.py <input> <output_dir>")
        sys.exit(1)
    process(sys.argv[1], sys.argv[2])
```

**Key patterns:**
- Docstring explaining purpose
- Input validation with clear errors
- Graceful directory creation
- Informative progress output

## Example: Validation Script (JavaScript)

```javascript
#!/usr/bin/env node
/**
 * Validate configuration files.
 */
const fs = require('fs');
const path = require('path');

function validate(configPath) {
  if (!fs.existsSync(configPath)) {
    console.error(`Error: ${configPath} not found`);
    process.exit(1);
  }

  try {
    const config = JSON.parse(fs.readFileSync(configPath, 'utf8'));
    console.log('Valid configuration');
    return config;
  } catch (e) {
    console.error(`Invalid JSON: ${e.message}`);
    process.exit(1);
  }
}

if (require.main === module) {
  if (process.argv.length !== 3) {
    console.log('Usage: validate.js <config.json>');
    process.exit(1);
  }
  validate(process.argv[2]);
}
```

## Production Examples

| Skill | Scripts | Pattern |
|-------|---------|---------|
| raindrop-skill | `raindrop.sh` | Bash API wrapper |
| anthropic/pdf | 8 Python scripts | File processing |

See [real-examples.md](real-examples.md) for full skill structures.
