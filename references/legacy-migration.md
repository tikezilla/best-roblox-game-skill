# Legacy Migration Guide

Use this reference when modernizing old Roblox code, repairing inherited projects, or checking deprecated APIs.

## Migration Rules

- Audit first, migrate second.
- Prefer incremental repairs over total rewrites.
- Do not migrate stable production data systems without a rollback plan.
- Check current official docs before changing marketplace, data, identity, ad, or platform APIs.
- Playtest after every coherent migration batch.

## Common Low-Risk Updates

| Legacy Pattern | Modern Replacement | Notes |
|---|---|---|
| `spawn(function() ... end)` | `task.spawn(function() ... end)` | Usually drop-in |
| `delay(seconds, fn)` | `task.delay(seconds, fn)` | Usually drop-in |
| `wait(seconds)` | `task.wait(seconds)` | Watch for local functions named `wait` |
| Parent first after `Instance.new` | Set properties first, then `Parent` | Avoids extra replication/events |
| camelCase API aliases | PascalCase documented methods | Example: `findFirstChild` -> `FindFirstChild` |

## Data Migration

ProfileStore is the preferred successor for player profiles. ProfileService-era code often maps directly but uses different lifecycle names:

| Older ProfileService Term | ProfileStore Term |
|---|---|
| `LoadProfileAsync` | `StartSessionAsync` |
| `Release` | `EndSession` |
| release listener | `OnSessionEnd` |
| `Reconcile` | `Reconcile` |

Raw DataStore code can be acceptable for prototypes, but it does not provide session ownership. Never call a raw `GetAsync`/`UpdateAsync` wrapper session locked unless it implements an actual ownership protocol.

## Detection Scans

Use script search/grep capabilities to find:

```text
spawn(
delay(
wait(
findFirstChild
getChildren
isA
DataStore2
ProfileService
PlayerOwnsAsset
UserHasBadge
MarketplaceService
AdService
Accoutrement
```

Read each match before editing. Some strings may be comments, documentation, or local helper names.

## Incremental Strategy

1. Inventory scripts and entry points.
2. Classify findings by risk: low, medium, high.
3. Apply low-risk task-library and casing updates in one batch.
4. Playtest and read console output.
5. Handle medium-risk API changes with feature-specific tests.
6. Treat data migration as its own project with staging, backup, and rollback notes.

## When Not To Migrate

- The project is close to a deadline and the legacy code works.
- The code belongs to a third-party package you do not own.
- The migration touches player data without a tested fallback.
- The user asked for a contained repair, not modernization.

When in doubt, use `references/current-platform-lookup.md` and report the risk before changing working code.