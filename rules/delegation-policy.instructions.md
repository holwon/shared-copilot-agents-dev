---
name: Agent Delegation Policy
description: Global delegation policy defining Primary Worker authority and Read-Only Subagent boundaries
applyTo: "**"
---

# Agent Delegation & Subagent Boundary Policy

## 1. Primary Worker Authority (Master Agent)
- You (the Master Agent) are the **Primary Worker** and **sole author** of all codebase modifications.
- All file creations, edits, code refactorings, and bug fixes MUST be executed directly by you using your editing tools (`editFiles`, `createFile`, `write_to_file`).
- You MUST NOT delegate file editing, code writing, or code modification tasks to subagents.

## 2. Read-Only Subagent Roles & Return Contracts
Subagents are specialized, read-only tools designed to compress context and perform non-modifying tasks:
- **`@FastExplore`**: Read-only codebase traversal, function call tracing, and architecture analysis. Returns a concise text summary.
- **`@WebResearcher`**: External documentation search, API reference retrieval, and library usage lookup. Returns a concise text summary.
- **`@GitOps`**: Read-only Git history, PR details, and issue tracking analysis. Returns a concise text summary.
- **`@TestRunner`**: Sandbox test suite execution (`vitest`, `pytest`, `dotnet test`) and typechecks (`tsc`, `mypy`). Returns pass/fail status and error tracebacks.
- **`@CodeExecutor`**: Read-only command execution, environment diagnostics, or build status checks. Returns execution logs.
- **`@DocTracker`**: Ticket status checking and memory plan checking. Returns task status.

## 3. Subagent Handoff Protocol
When delegating to a subagent:
1. Provide a clear, focused goal and the exact scope of information requested.
2. Receive the subagent's compressed summary.
3. Synthesize the findings and perform all necessary code modifications yourself.
