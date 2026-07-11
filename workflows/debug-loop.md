# Iterative Debugging Workflow

Use this workflow for Roblox bugs, console errors, broken gameplay, UI failures, and MCP-assisted repair.

## Step 1: Gather Evidence

- Read current console output.
- If a playtest is active, capture runtime output before stopping it.
- Record exact error text, script path, line number, client/server origin, and reproduction steps.
- If visual behavior is wrong, capture a screenshot when supported.

## Step 2: Locate Source

Use available capabilities:

- Script search by name or error text
- Script grep for function names, RemoteEvents, or missing instance names
- Script read for the failing source and callers
- Game tree search for physical Workspace/UI dependencies
- Luau execution for inspection only when direct tools are insufficient

Read affected scripts before editing.

## Step 3: Diagnose

Classify the root cause:

| Category | Examples |
|---|---|
| Syntax | missing `end`, malformed table, invalid type annotation |
| Runtime | nil reference, missing Instance, invalid service, WaitForChild timeout |
| Logic | wrong condition, bad math, event order bug |
| Networking | client/server mismatch, missing RemoteEvent, unvalidated payload |
| Persistence | failed load, save race, schema mismatch, missing ProfileStore session |
| UI | bad hierarchy, unsafe layout, blocked input, text overflow |
| Performance | runaway loop, leaked connection, too many objects |

## Step 4: Fix Coherently

- Apply the smallest complete fix that addresses the root cause.
- Preserve the existing architecture unless replacement has a concrete benefit.
- Avoid unrelated refactors.
- Add guards, validation, or missing instances with clear ownership.
- Create an undo waypoint when available before risky edits.

## Step 5: Verify

1. Check immediate console output after the edit.
2. Start playtest if runtime behavior is involved.
3. Trigger the reproduction path.
4. Read console output again.
5. Capture screenshot evidence for UI/visual fixes.
6. Stop playtest before further edit-mode changes.

## Step 6: Iterate Or Report

Repeat up to five repair cycles. If the issue persists, report:

- Each hypothesis and fix attempted
- Current exact error or behavior
- Scripts and instances inspected
- Most likely remaining cause
- Next manual diagnostic step

## Success Summary

When fixed, summarize:

```text
Bug fixed:
Root cause:
Files/scripts changed:
How it was verified:
Preventive note:
```