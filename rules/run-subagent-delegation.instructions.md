---
name: Subagent Delegation Rule
description: "MANDATORY: use the runSubagent tool whenever a task requires delegated investigation or subagent-style workflow. This is a focused supplement to the main delegation policy."
applyTo: "**"
---

# Subagent Delegation Rule

This instruction is a focused supplement to [delegation-policy.instructions.md](delegation-policy.instructions.md).

## Mandatory requirement

When the task involves any delegated investigation or analysis, the agent must use the `runSubagent` tool instead of doing the work directly in the current context.

This includes, but is not limited to:

- codebase exploration
- symbol lookup and call tracing
- architecture analysis
- external documentation lookup
- Git history or blame inspection
- test execution and diagnostic investigation
- any task that should be performed by a child/subagent

## Subagent behavior

- Treat the delegated worker as a child/subagent, not as a direct inline action.
- Use a focused prompt that states the exact target, scope, and expected output.
- Request a compressed summary, not verbose raw output.
- Integrate the result into the current task, then continue the work in the main flow.

## Prohibited behavior

- Do not skip `runSubagent` for investigation tasks that are meant to be delegated.
- Do not perform codebase exploration, lookup, or tracing directly when the task matches the subagent workflow.
- Do not treat a delegated subagent as an optional convenience; it is required for these tasks.

## Relationship to the main policy

This rule does not replace the general delegation policy. It strengthens it by explicitly requiring the use of `runSubagent` for subagent-based work.
