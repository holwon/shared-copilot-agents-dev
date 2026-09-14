---
name: Agent Output Hygiene
description: "Referenced by terminal-running subagents (GitReader, GitOps, CodeExecutor, TestRunner). Prevents tool-output spill loops when a tool result returns a file path instead of its content."
---

# Agent Output Hygiene

Applies to every agent and subagent. Running **many** queries is expected and encouraged — bounding each result and never looping is mandatory.

## 1. The spill-file contract (mandatory)

When a tool result exceeds the inline limit, the host returns **the path of a file** instead of the content, e.g.:

```text
.../GitHub.copilot-chat/chat-session-resources/<session>/call_<id>__vscode-<ts>/content.txt
```

- Read that file with the **file-read tool** (`read_file`).
- **NEVER** read it with a shell command — `Get-Content`, `cat`, `type`, `head`, `tail`, `more`, `Select-String`.
  The shell output spills again → a new file → a new read → **infinite loop**.
- **NEVER** re-run the original command to "see more output". Same command, same result, same spill.

## 2. Bound each result at the source

- **Git**: always `--no-pager --no-color`. Bound with `-n <N>`, `--oneline`, `--stat`, `--name-only`, `-U0`, or a path/range filter.
- Prefer **several bounded calls** over one mega-blob. Do **not** chain many commands with `;` or `&&` — the merged output is one unattributable blob and is the easiest thing to overflow.
- The goal is that no *single* result exceeds the limit — **not** that you run few queries. Absorbing many noisy queries and returning a small summary is the entire point of a subagent.

## 3. Degrade, don't loop

- **No file-read tool available?** Report the truncation to the caller and request a narrower scope. Do **not** attempt a shell workaround.
- **Two identical failing attempts = stop** and report. Never retry the same command a third time.
