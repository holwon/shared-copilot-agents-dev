---
name: codebase-memory
description: 'MANDATORY first-call workflow for the Codebase Memory MCP knowledge graph. Use BEFORE reading files or editing code on any request touching this codebase — call list_projects, get_architecture, search_graph, trace_call_path, then read_file only for exact line edits. Triggers on: explore the codebase, understand the architecture, what functions exist, show me the structure, who calls this function, what does X call, trace the call chain, find callers of, show dependencies, impact analysis, dead code, unused functions, high fan-out, refactor candidates, code quality audit, graph query syntax, Cypher query examples, edge types, how to use search_graph.'
argument-hint: 'Describe WHAT you need from the codebase (architecture, symbol, call chain, impact, etc.)'
user-invocable: false
disable-model-invocation: false
---

# Codebase Memory MCP — First-Call Workflow

## Rule (Non-Negotiable)

**For every request involving this codebase, call the Codebase Memory MCP graph tools FIRST — before `read_file` and before any code change.**

The knowledge graph is the fast path: it answers structure, symbol, and call-chain questions in one or two tool calls instead of dozens of file reads. Only fall back to `read_file` when you need the exact raw bytes of a specific line to edit it.

## When to Use

- Explore a codebase / understand its architecture
- Find functions, classes, symbols, or endpoints
- Trace call chains (`who calls X`, `what does X call`)
- Impact analysis ("if I change this, what breaks?")
- Dead-code / unused-symbol / high-fan-out audits
- Refactor candidate discovery
- Any Cypher / graph-schema / `search_graph` question

## Workflow

### Step 0 — Discover the project name

If you do not already know the exact project identifier, call `list_projects` first. Use the `display_name` or exact `name` it returns in all later calls.

```json
mcp_codebase-memo_list_projects()
```

### Step 1 — Load the architecture overview

```json
mcp_codebase-memo_get_architecture({ "project": "<display_name>" })
```

This returns languages, packages, routes, and hotspots — your mental map before any file read.

### Step 2 — Search and trace

- **Find symbols**: `search_graph(name_pattern, name_scope, label, file_pattern, exclude_file_pattern)`
- **Trace calls**: `trace_call_path(function_name, direction, depth)` — `direction` is `incoming`/`outgoing`, BFS up to `depth` hops
- **Ad-hoc graph query**: `query_graph("<cypher-like read-only query>")`
- **Schema**: `get_graph_schema(project)` for node/edge counts and relationship patterns
- **Text grep**: `search_code(pattern, project)` when you only have a string, not a symbol

### Step 3 — Read implementation snippets

```json
mcp_codebase-memo_get_code_snippet({ "qualified_name": "<qualified_name>" })
```

Use this to read a function's source without opening the whole file.

### Step 4 — Edit with `read_file` only when needed

Only now, if you must change a specific line, call `read_file` with the exact `file_path` + `startLine`/`endLine` to get raw content for an edit. Do **not** use `read_file` for discovery.

## Available Tools (14 MCP tools)

**Indexing**
- `index_repository(repo_path)` — Index a repository into the knowledge graph
- `list_projects` — List all indexed projects with node/edge counts
- `delete_project(project)` — Remove a project and all its graph data
- `index_status(project)` — Check indexing status

**Querying**
- `search_graph(name_pattern, name_scope, label, file_pattern, exclude_file_pattern)` — Structured search by label, name/qualified_name, include/exclude file globs
- `trace_call_path(function_name, direction, depth)` — BFS call chain traversal
- `detect_changes(project)` — Map git diff to affected symbols + risk
- `query_graph(query)` — Execute Cypher-like graph queries (read-only)
- `get_graph_schema(project)` — Node/edge counts, relationship patterns
- `get_code_snippet(qualified_name)` — Read source code for a function
- `get_architecture(project)` — Codebase overview: languages, packages, routes, hotspots
- `search_code(pattern, project)` — Grep-like text search within indexed files
- `manage_adr(action)` — CRUD for Architecture Decision Records
- `ingest_traces(traces)` — Ingest runtime traces to validate HTTP edges

## Decision Guide

| Need | Tool |
|------|------|
| "What projects are indexed?" | `list_projects` |
| "Give me the lay of the land" | `get_architecture` |
| "Where is class `Foo`?" | `search_graph` with `name_pattern` |
| "Who calls `bar()`?" | `trace_call_path` `direction=incoming` |
| "What does `baz()` call?" | `trace_call_path` `direction=outgoing` |
| "Show me the schema" | `get_graph_schema` |
| "Read `qux()` source" | `get_code_snippet` |
| "Edit line 42 of `a.ts`" | `read_file` (exact range) → edit |

## Anti-Patterns

- **Reading files before graphing** — defeats the purpose; wastes context and calls.
- **Guessing the project name** — always `list_projects` first if unsure; a wrong `project` arg fails silently or returns empty.
- **Using `read_file` for discovery** — use `search_graph` / `get_code_snippet` instead.
- **Skipping `trace_call_path` for impact analysis** — graph traversal is exact; grep is lossy.
