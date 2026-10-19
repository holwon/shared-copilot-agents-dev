---
name: Code Usage-First Search
description: "Code research habit — prefer symbol-aware usages (vscode_listCodeUsages: definitions, implementations, callers) over blind keyword search. Use usages aggressively when understanding or refactoring code."
applyTo: "**"
---

# Code Usage-First Search

When you already hold a symbol (any definition, call site, or import), reach for `vscode_listCodeUsages` first — it resolves definitions, implementations, and every reference.

Fall back to text search only when you have no symbol: hunting a string literal, a log message, a config key, a comment, or files by glob.