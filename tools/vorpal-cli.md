# Vorpal CLI

Command-line interface for the Vorpal Engineering knowledge graph — a curated dataset of smart contract security patterns, vulnerabilities, exploits, mitigations, protocols, languages, standards, and tools.

This document is a reference for skills (and humans) that need to query the knowledge base.

## Installation

```bash
brew install vorpalengineering/tap/vorpal
```

## Configuration

Get an API key from the Vorpal console (https://console.vorpalengineering.com → Access page), then:

```bash
vorpal config set --api-key <key>
```

Optional: point at a custom API endpoint with `--api-url <url>`.

Verify configuration:

```bash
vorpal config
```

## Commands

| Command | Purpose |
|---|---|
| `vorpal knowledge search <text>` | Find nodes matching a query (keyword or semantic) |
| `vorpal knowledge list` | Paginated browse of all nodes |
| `vorpal knowledge get <id>` | Full node details + related nodes |
| `vorpal knowledge traverse <id>` | Walk the knowledge graph from a starting node |
| `vorpal knowledge types` | List available node and edge types |

Always pass `--json` for structured output.

---

### `vorpal knowledge search <text>`

Find nodes matching a query.

**Flags:**
- `--mode keyword|semantic` — search mode (default: `keyword`)
  - **keyword**: exact-term full-text match. Fast and precise.
  - **semantic**: embedding-based similarity search. Use for conceptual queries where wording may differ from indexed content.
- `--threshold N` — similarity threshold for semantic only (default 0.5, range 0-2, lower = stricter match). Ignored without `--mode semantic` (CLI emits a warning).
- `--type <name>` — filter by node type (e.g., `concept`, `exploit`, `protocol`). Run `vorpal knowledge types --nodes` to discover valid values.
- `--limit N` — max results (default 5, max 20)
- `--json` — structured output

**Examples:**

```bash
# Keyword search (default)
vorpal knowledge search --json "reentrancy"

# Semantic search with strict threshold
vorpal knowledge search --json --mode semantic --threshold 0.3 "oracle price manipulation flash loan"

# Filter to concepts only
vorpal knowledge search --json --type concept "access control"
```

**JSON response:**

```json
{
  "results": [
    {
      "id": "uuid",
      "node_type": "concept",
      "title": "Cross-Function Reentrancy",
      "preview": "First N chars of the node details...",
      "quality": 5
    }
  ]
}
```

---

### `vorpal knowledge list`

Paginated browse of all nodes, optionally filtered by type.

**Flags:**
- `--limit N` (default 10)
- `--offset N` (skip N entries)
- `--type <name>` — filter by node type
- `--json`

**Example:**

```bash
vorpal knowledge list --json --type exploit --limit 20
```

---

### `vorpal knowledge get <id>`

Fetch a single node with its full details, citations, and related nodes.

**Flags:**
- `--json`

**Example:**

```bash
vorpal knowledge get <id> --json
```

**JSON response:**

```json
{
  "id": "uuid",
  "node_type": "concept",
  "title": "Cross-Function Reentrancy",
  "details": "Full description...",
  "citations": "[{\"label\":\"SWC-107\",\"url\":\"...\"}]",
  "quality": 5,
  "data": { "category": "vulnerability", "context": "solidity" },
  "created_at": "2026-...",
  "related": [
    {
      "id": "uuid",
      "node_type": "concept",
      "edge_type": "mitigates",
      "title": "Checks-Effects-Interactions Pattern",
      "quality": 5,
      "direction": "inbound"
    }
  ]
}
```

`direction` is one of:
- `outbound` — this node points at the related node
- `inbound` — the related node points at this node
- `symmetric` — bidirectional relationship (e.g., `related`)

---

### `vorpal knowledge traverse <id>`

Walk the knowledge graph from a starting node to discover indirect relationships.

**Flags:**
- `--depth N` (1-5, default 2) — how many hops to walk
- `--json`

**Example:**

```bash
vorpal knowledge traverse <id> --depth 2 --json
```

**JSON response:**

```json
{
  "nodes": [
    { "id": "uuid", "node_type": "concept", "title": "...", "quality": 5 }
  ],
  "edges": [
    { "source_id": "uuid", "target_id": "uuid", "edge_type": "mitigates", "quality": 5 }
  ]
}
```

Useful for multi-hop reasoning — e.g., "what exploits relate to a vulnerability that affects this protocol?"

---

### `vorpal knowledge types`

List all node types and edge types in the knowledge graph.

**Flags:**
- `--nodes` — only show node types
- `--edges` — only show edge types
- `--json`

(No flags = show both.)

**Example:**

```bash
vorpal knowledge types --json
```

Use this to discover what `--type` values are valid for `search` and `list`, and what `edge_type` values to expect from `get`/`traverse` responses.
