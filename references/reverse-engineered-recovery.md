# Reverse-Engineered Project Recovery

Use this reference for old, inherited, decompiled, mangled, duplicated, or partially rewritten Roblox projects. The goal is behavioral recovery: reconstruct what the game should do, then implement that behavior behind the current project boundaries.

## Evidence order

Prefer evidence in this order, while recording uncertainty:

1. Current runtime behavior, Studio output, and active entry points.
2. Current client/server call sites, remote contracts, data schemas, and service boundaries.
3. Related active modules and data tables that agree with one another.
4. Legacy or decompiled scripts, old names, serialized records, and duplicate implementations.
5. Generic Roblox patterns or guesses.

Tag conclusions as:

- `Observed`: directly visible in active code or confirmed at runtime.
- `Inferred`: strongly supported by multiple call sites, data, or sibling systems.
- `Speculative`: plausible but not confirmed because code, assets, or runtime evidence is missing.

Never present a speculative behavior as recovered fact. Do not implement speculative behavior in a critical path until a contained test, asset inspection, or user decision confirms it.

## Recovery method

1. Inventory the tree by execution boundary: `ServerScriptService`, `ServerStorage`, `ReplicatedStorage`, `StarterPlayer`, `StarterGui`, and `Workspace`.
2. Find active bootstraps and require/load order. Mark `.legacy`, `[OLD]`, duplicate, and decompiled files as evidence-only until proven active.
3. Build a contract ledger for each behavior: caller, remote or bindable name, payload fields, return values, cooldown, validation, state mutation, persistence, and client feedback.
4. Trace a complete path from UI or input to client request, remote bridge, server handler, service, state mutation, save, and response/event.
5. Compare modern and legacy versions by behavior, not by line count. Recover missing rules, validation, defaults, and side effects without copying damaged control flow.
6. Use invariants to resolve gaps: ownership, permissions, plot bounds, currency conservation, idempotence, valid state transitions, and save/load round trips.
7. Cross-check names and shapes across client code, data tables, serialized records, and sibling handlers. Preserve odd names when they are part of an existing contract.
8. Choose one contained repair or vertical slice. Keep unrelated unknowns documented rather than silently “fixing” them.
9. Implement behind the current canonical service or module boundary. Add a compatibility adapter only when an existing caller cannot be migrated safely.
10. Verify both the happy path and rejection paths, then update the confidence tags with the observed result.

## Rewritten server backend

When the project has a new backend rewritten from scratch:

- Treat the rewritten server services and their public router as the authority for ownership, validation, state changes, and persistence.
- Treat decompiled server scripts as a behavioral oracle. Mine them for missing actions, defaults, side effects, and historical intent; do not restore them wholesale.
- Treat every client request as hostile and incomplete. Validate type, shape, ownership, plot or distance bounds, permission, cost, cooldown, current state, and idempotence on the server.
- Let the server calculate prices, rewards, inventory changes, placement results, and save records. The client may request or preview an action, but must not author its outcome.
- Reconcile both sides of every contract. A registered server action with no client caller may be dead code; a client action with no handler is a real recovery lead.
- Preserve the current remote bridge and service boundaries when they are working. Do not reintroduce old remotes merely because a legacy script used a different route.
- If a handler uses `pcall`, log the action and return a safe failure result. Do not turn a missing dependency into a silent success.
- Check cleanup on player removal, plot release, session end, and failed requests. Leaked ownership or cooldown state can look like a data bug.

## Historical-fidelity projects

For a recreation targeting an older game era, separate historical fidelity from current platform compatibility:

- Reproduce the intended behavior, content relationships, and player loop of the target era.
- Use current Roblox APIs and secure server patterns required to run the project today.
- Do not invent modern systems just because they are convenient if they change the historical behavior being reconstructed.
- Preserve compatibility-sensitive action strings, folder names, serialized formats, and client-visible timing when call sites depend on them.
- Prioritize the core loop and its contracts: plot/house ownership, build placement, interactions, needs or moods, jobs, economy, inventory, and save/load.

## Contract ledger

Use a compact table while investigating:

| Behavior | Caller | Server authority | State/persistence | Confidence | Gap |
|---|---|---|---|---|---|
| Example action | UI/module | service/handler | profile/plot/runtime | Observed/Inferred/Speculative | next evidence |

For each unresolved gap, write the smallest evidence-gathering step that could resolve it. Prefer reading one more call site or running one focused playtest over adding a broad fallback.
