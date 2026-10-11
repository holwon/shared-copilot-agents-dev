---
name: Terminal Discipline
description: "Shared execution contract for every agent that runs a terminal command: one plain command per call (never a fused shell blob), and the closed-loop handling of a hung or unresponsive command."
---

# Terminal Discipline

Applies to every agent that runs a terminal command. The concern is **execution**, not reading (for reading a spilled result, see [agent-output-hygiene](agent-output-hygiene.instructions.md)). Two failure modes drive this: a fused shell blob wedges the terminal, and a stalled command has no exit without a probe.

## 1. One plain command per call

- **One command per terminal call.** Each call runs exactly one plain command — no pipes (`|`), redirects (`>`, `>>`, `2>&1`), chaining (`;`, `&&`, `||`), loops (`for`, `while`), backgrounding (`&`), or wrapping (`bash -lc '…'`).
- **Serial, never fused.** A phase of N commands is N calls, never one compound blob — a fused blob seizes the terminal, blends every command's output into one unattributable lump, and is the usual cause of a wedged session.
- **Run only the caller's commands.** Never invent probing or diagnostic commands (`echo`, `date`, `ls`, `ps`, `pkill`, "is the terminal alive?"). A stalled command is handled by §2, not by a probe.
- **Verbatim.** Never modify the caller's command.

## 2. When a command hangs (closed loop)

Every branch ends with a recorded result, never a new probe.

- **Synchronous** (result inline) → capture, distill, next command.
- **Moved to background** → read with `getTerminalOutput`, at most **2 attempts**:
  - Produced output and is **meant to keep running** (a server / watcher) → record its Terminal ID, mark `RUNNING (background)`, continue.
  - Produced **nothing** (it should have finished but stalled) → `killTerminal`, mark `FAILED (hung)`, stop the phase, report.
- **Terminal unresponsive / no longer available** → mark `FAILED (terminal unresponsive)`, stop the phase, report. Do not spawn a replacement shell.

A **non-zero exit code is not a hang** — it is a normal result. Record it and continue.
