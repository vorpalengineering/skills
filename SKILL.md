---
name: vorpal-knowledge-base
description: Searches the Vorpal Engineering knowledge base for smart contract security patterns, vulnerabilities, and best practices using the vellma CLI. Use when auditing Solidity code, investigating vulnerabilities, reviewing smart contracts, or when the user asks about common security patterns like reentrancy, access control, oracle manipulation, or gas optimization.
---

# Vorpal Knowledge Base

Search a curated knowledge base of smart contract security patterns, vulnerabilities, and mitigations via the `vellma` CLI.

## Prerequisites

The `vellma` CLI must be installed and configured with an API key.

Check if available:

```bash
vellma auth status
```

If not installed, the user needs to:
1. Install: `brew install vorpalengineering/tap/vellma`
2. Get an API key from the Vorpal console (https://console.vorpalengineering.com → Access page)
3. Configure: `vellma auth set-key <key>`

## Searching the Knowledge Base

Use semantic search to find relevant security patterns:

```bash
vellma knowledge search --json "reentrancy vulnerability"
```

Always use `--json` for structured output. Use `--limit` to control result count and `--threshold` for relevance filtering:

```bash
vellma knowledge search --json --limit 3 --threshold 0.3 "oracle price manipulation flash loan"
```

- `--limit N` — max results (default 5, max 20)
- `--threshold N` — similarity threshold (default 0.5, lower = stricter match, range 0-2)

### JSON response format

```json
{
  "results": [
    {
      "title": "Cross-Function Reentrancy",
      "category": "Reentrancy",
      "severity": "critical",
      "content": "Description of the vulnerability...",
      "code_examples": "// Vulnerable\nfunction withdraw()...\n\n// Fixed\nfunction withdraw()...",
      "mitigation": "Follow checks-effects-interactions pattern...",
      "quality": 3
    }
  ]
}
```

## When to Use

- **During code review**: Search for patterns matching the code being reviewed
- **Investigating a finding**: Look up known vulnerability patterns related to a specific finding
- **Writing audit reports**: Reference established vulnerability descriptions and mitigations
- **Learning about a vulnerability class**: Search by topic or severity

## Example Workflow

When auditing a contract that makes external calls:

```bash
# Search for relevant patterns
vellma knowledge search --json "external call state update reentrancy"

# If reviewing access control
vellma knowledge search --json "access control selfdestruct owner"

# Strict search for highly relevant results only
vellma knowledge search --json --threshold 0.3 "flash loan oracle manipulation"
```

Parse the JSON output to extract relevant titles, descriptions, code examples, and mitigations. Present findings to the user with the knowledge base context.
