# Workflow: Existing Project Recovery

Use this workflow for old, broken, inherited, interdependent, decompiled, or partially rewritten Roblox projects. For deeper behavior inference, pair it with `references/reverse-engineered-recovery.md`.

## Hard Rule

Do not respond to a broken inherited project by proposing a total rewrite before producing a dependency map and testing one contained repair.

## Recovery Steps

1. Inventory all scripts, modules, remotes, and Workspace dependencies.
2. Find server and client entry points.
3. Map require chains.
4. Map RemoteEvent and RemoteFunction traffic.
5. Identify missing physical instances.
6. Separate working, broken, unused, and unknown systems.
7. Pick one playable vertical slice.
8. Repair that slice without rewriting unrelated systems.
9. Test it.
10. Record recovered contracts, confidence tags, and the next evidence needed for unresolved behavior.

## Inventory Checklist

- `ServerScriptService` scripts and services
- `ReplicatedStorage` modules, remotes, and shared assets
- `StarterPlayerScripts` and `StarterCharacterScripts`
- `StarterGui` ScreenGuis and LocalScripts
- `Workspace` named objects referenced by scripts
- `ServerStorage` hidden assets and modules
- DataStore/ProfileStore ownership
- Third-party packages and suspicious inserted assets

## Authority Rules

- Treat active current services and runtime behavior as canonical.
- Treat `.legacy`, `[OLD]`, duplicate, and decompiled scripts as evidence until their active load path is proven.
- When a backend has been rewritten, recover missing behavior behind the new service boundary instead of restoring the old server script wholesale.
- Preserve existing remote names, payload shapes, object names, and serialized formats when current callers depend on them.

## Report Format

```text
Recovered Project Map
- Entry points:
- Core systems:
- Remotes:
- Module dependencies:
- Workspace dependencies:
- Working systems:
- Broken systems:
- Unused/unknown systems:
- First repair slice:
- Reusable ideas/assets:
```

## Repair Rules

- Read every affected script before editing it.
- Prefer one contained repair over broad refactors.
- Add missing physical instances only after proving scripts expect them.
- Preserve names that existing code references.
- Playtest the repaired slice and read console output.
- Stop after five repair cycles and report the remaining blocker.
