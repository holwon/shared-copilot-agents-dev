---
name: Code Usage-First Search
description: "Code research habit — prefer symbol-aware usages (vscode_listCodeUsages: definitions, implementations, callers) over blind keyword search. Use usages aggressively when understanding or refactoring code."
applyTo: "**"
---

# Code Usage-First Search

## Principle

When researching, understanding, or refactoring code, PREFER the usages tool over blind global keyword search. `search/usages` (a.k.a. `vscode_listCodeUsages` / `vscodeGeneral/usages`) resolves references, implementations, and callers precisely; a keyword search returns raw matches that may point at a different symbol entirely.

## Tool mapping

- **usages** (`search/usages` ≡ `vscode_listCodeUsages`): the symbol-aware query — definitions, implementations, all callers/references, rename impact, call-chain tracing. This is the DEFAULT for code understanding.
- **textSearch / fileSearch** (`search/textSearch`, `search/fileSearch`): raw keyword / glob matching — only when you do NOT have a symbol, or are hunting a string literal, comment, log line, or config key.

## Decision rule

| Goal | Use |
|---|---|
| "who calls X", "where is X defined/implemented", "X rename impact", "trace the call chain" | **usages** (preferred) |
| "where does word W / string S appear", "files matching glob G", symbol unknown | textSearch / fileSearch |

## Be aggressive

- When you hold ANY occurrence of a symbol (you can point at one source location), run usages from that point instead of guessing with grep. Always try `vscode_listCodeUsages` first.
- Favor several targeted usages calls over one broad textSearch — precision over recall. Fall back to textSearch only when the symbol name itself is unknown.
- Reserve textSearch for genuinely keyword-shaped questions; do not fall back to "search for the name" while usages is available.

## Anti-patterns

- Using textSearch to "find usages of X" when you already hold a valid reference to X in hand — that is exactly what usages is for.
- Digging through raw keyword matches when a symbol-resolved query would have answered cleanly.
- Reaching for a broad global search as the first move in any code-understanding task.