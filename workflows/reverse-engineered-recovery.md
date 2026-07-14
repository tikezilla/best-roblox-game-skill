# Workflow: Reverse-Engineered Recovery

Use this workflow when an existing Roblox experience contains decompiled, mangled, duplicated, legacy, or partially rewritten scripts and the intended behavior must be inferred from context.

## 1. Orient

- Identify the writable Studio session or filesystem checkout.
- Inventory scripts, modules, remotes, data folders, assets, and bootstraps.
- Separate active files from `.legacy`, `[OLD]`, duplicate, generated, and reference-only files.
- Read the relevant current scripts before editing any of them.

## 2. Establish authority

Create an authority map:

| Layer | Default role |
|---|---|
| Current server router/services | Canonical server behavior and validation |
| Current shared modules | Shared data, constants, deterministic helpers |
| Current client modules | Input, previews, UI, prediction, local effects |
| Legacy/decompiled scripts | Evidence for missing intent and historical behavior |
| Runtime output and playtest | Confirmation or contradiction of a hypothesis |

If the current backend was rewritten, do not weaken it to match a damaged legacy implementation. Recover the behavior behind the current service boundary.

## 3. Map one behavior end to end

For the requested feature, record:

1. Client entry point and user action.
2. Remote, bindable, or module call name.
3. Payload fields and return/event shape.
4. Server handler and service delegation.
5. Ownership, permission, range, type, cost, cooldown, and state checks.
6. State mutation and persistence record.
7. Client feedback, UI update, animation, sound, or notification.
8. Legacy evidence and the confidence tag for each conclusion.

Do not patch a missing server function until its caller, side effects, and failure behavior are understood.

## 4. Form hypotheses

For every gap, list:

- `Observed`: what the current code proves.
- `Inferred`: what multiple sources strongly imply.
- `Speculative`: what remains a guess.
- `Next check`: the smallest read or playtest that can raise confidence.

Prefer the hypothesis that preserves existing contracts and invariants. When two hypotheses remain viable, do not hide the ambiguity inside a broad fallback; choose a contained implementation or report the decision point.

## 5. Implement the smallest repair

- Add or repair behavior in the current canonical module/service.
- Port rules and side effects from legacy code, not its damaged structure.
- Keep the server authoritative for money, inventory, plot ownership, build placement, permissions, jobs, and persistence.
- Preserve existing names and payload shapes unless all callers are updated together.
- Avoid unrelated refactors, broad deletion, or restoring an entire legacy script.

## 6. Verify

Test the repaired slice with:

- Valid request from the intended caller.
- Invalid type, missing field, wrong owner, out-of-range target, insufficient funds, and repeated request.
- Player leave, plot release, failed save, and rejoin when persistence is involved.
- Current Studio output after the change.
- A focused playtest and screenshot when UI or world state is part of the behavior.

After verification, update the behavior map and confidence tags. If five focused repair cycles do not resolve the issue, report the exact blocker and the next evidence needed.

## Server-backend review checklist

- [ ] Every client-used action has exactly one intentional server path.
- [ ] Every server path has a known caller or a documented compatibility reason.
- [ ] Remote payloads are type-checked and normalized before use.
- [ ] Plot, house, object, inventory, job, and player ownership are checked server-side.
- [ ] Costs and rewards are calculated from server data.
- [ ] State transitions are valid and idempotent where retries are possible.
- [ ] Persistence writes use the current schema and preserve migration boundaries.
- [ ] Errors fail closed, are logged with action context, and do not claim success.
- [ ] Player/plot/session cleanup clears temporary state.
- [ ] Legacy code is cited as evidence and not reintroduced wholesale.

## Recovery report

```text
Recovered Behavior
- Target slice:
- Active entry point:
- Client request/event:
- Server handler/service:
- State and persistence:
- Legacy evidence:
- Observed:
- Inferred:
- Speculative:
- Missing instance or contract:
- Repair made:
- Verification:
- Remaining gap:
```
