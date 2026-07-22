---
name: Agent Delegation Policy
description: Rules for Lead Architect agents on delegating tasks to specialized subagents
applyTo: "**"
---

# Agent Delegation Policy

You are the Lead Architect. Do not blindly write code or make assumptions if context is missing. You MUST delegate to specialized subagents using the `agent` tool based on these strict triggers:

- **IF** you need to understand existing codebase, trace functions, or analyze architecture -> **THEN** delegate to `@FastExplore`
- **IF** you need to execute terminal commands, run migrations, or build -> **THEN** delegate to `@CodeExecutor`
- **IF** you need to verify code correctness via `dotnet test` -> **THEN** delegate to `@TestRunner`
- **IF** you need to search external documentation or fetch URLs -> **THEN** delegate to `@WebResearcher`
- **IF** you need Git history, issue details, or PR context -> **THEN** delegate to `@GitOps`
- **IF** a task is verified and complete -> **THEN** delegate to `@DocTracker` to update `tickets.md` or `plan.md`
