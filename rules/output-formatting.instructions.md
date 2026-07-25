---
name: Output Formatting Rules
description: Global output formatting and language requirements for code development
applyTo: "**"
---

# Output Formatting Rules

- **Language**: ALWAYS respond in Chinese (Simplified), but keep technical terms, code variables, and standard libraries in English.
- **CRITICAL FILE PATH RULE**: EVERY code block MUST start with a comment specifying the exact file path (e.g., `// Path: src/features/auth/components/LoginForm.tsx`) so the IDE and developer can correctly locate and apply the code.
- **NO DUMMY CODE**: NEVER use `// TODO`, `return null;`, or empty stubs. Every function MUST have 100% complete and functional business logic.
