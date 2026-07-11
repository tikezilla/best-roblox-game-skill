---
name: roblox-game
description: Expert Roblox development skill for Codex and compatible agents. Use for Roblox Studio, Luau, MCP-assisted game building, debugging, UI/UX polish, data persistence, competition projects, existing project recovery, security, performance, monetization, file formats, assets, Rojo, DataStoreService, ProfileStore, RemoteEvent, RemoteFunction, ServerScriptService, ReplicatedStorage, StarterGui, Roblox Engine APIs, and Congressional App Challenge Roblox projects. Do not use for Unity, Unreal, Godot, web apps, or non-Roblox scripting unless the user is explicitly comparing them to Roblox.
---

# Roblox Game Development Skill

Use this skill as a Codex-first Roblox development companion. Prefer the existing project shape, inspect before changing, and build working vertical slices that the user can test and explain.

## Roblox Studio MCP Detection

Inspect the Roblox Studio MCP tools exposed in the current session. Do not assume a server implementation, fixed tool count, or fixed naming scheme.

Map available tools into these capabilities:

1. Studio session management
2. Game tree search
3. Instance inspection
4. Script search and reading
5. Script editing
6. Luau execution
7. Asset insertion and generation
8. Playtest control
9. Console inspection
10. Input simulation
11. Screenshot capture

Use the actual tool names exposed in the session. Only enter offline mode when no Roblox Studio MCP tools are available. A missing preferred tool does not imply offline mode. Adapt through another available capability.

### Multiple Roblox Studio Sessions

When multiple Studio sessions are open:

1. Identify the intended target session.
2. Set that session as active before making changes.
3. Treat every other session as read-only reference material.
4. Never edit, insert, delete, or run destructive Luau in a reference session.
5. State which session is writable before the first modification.

## Routing Table

Match user intent and load the corresponding files before generating code or modifying Studio.

| User Intent | Load |
|---|---|
| Build a new game or prototype | `workflows/new-game.md` + `templates/game-scaffold.md` |
| Build a known genre system | `workflows/new-game.md` + `templates/genre-{type}.md` + `templates/game-scaffold.md` |
| Congressional App Challenge project | `workflows/congressional-app-challenge.md` + `references/visual-direction-ux.md` |
| Recover or understand an existing game | `workflows/existing-project-recovery.md` + `references/mcp-orchestration.md` |
| Fix bug / debug | `workflows/debug-loop.md` + `references/mcp-orchestration.md` |
| Professional UI, UX, animation, or visual identity | `references/gui-systems.md` + `references/visual-direction-ux.md` |
| Current or uncertain Roblox API | `references/current-platform-lookup.md` |
| Save/load data, DataStore, ProfileStore | `references/datastore-persistence.md` |
| Legacy migration or deprecated APIs | `references/legacy-migration.md` + `references/current-platform-lookup.md` |
| Roblox file formats, imports, exports, assets | `references/file-formats-and-assets.md` |
| Combat system | `references/combat-systems.md` + `references/security-hardening.md` |
| Shop, gamepass, or monetization | `references/monetization-systems.md` + `references/gui-systems.md` |
| Optimize performance | `workflows/performance-audit.md` + `references/performance-optimization.md` |
| Security review | `workflows/security-audit.md` + `references/security-hardening.md` |
| Set up Rojo or external tools | `references/tooling-ecosystem.md` |
| Gotchas, sharp edges, common bugs | `references/sharp-edges.md` |
| General Luau question | `references/luau-mastery.md` |
| Game design question | `references/game-design-roblox.md` |
| Ready to publish | `workflows/publish-checklist.md` |
| Review monetization | `workflows/monetization-audit.md` |
| Review code quality | `workflows/code-review.md` |
| Competition polish audit | `workflows/competition-polish-audit.md` + `references/visual-direction-ux.md` |
| Animation / VFX | `references/animation-vfx.md` |
| Multiplayer / networking | `references/multiplayer-networking.md` |
| Testing | `references/testing-patterns.md` |
| Inventory / items | `references/inventory-systems.md` |

If intent is ambiguous, ask one clarifying question, then route. If the user names a competition, deadline, judging, school project, civic app, or demo video, prefer the Congressional App Challenge route.

## Operating Rules

- Inspect the game tree and existing scripts before proposing architecture.
- Preserve working systems unless replacement has a concrete benefit.
- Build one playable vertical slice before expanding scope.
- Apply coherent batches of changes rather than isolated fragments.
- Test every completed feature in Studio when MCP capabilities allow it.
- Read console output after every playtest.
- Visually inspect UI and 3D work with screenshots when supported.
- Ask for approval before deleting substantial work, replacing architecture, or changing the core product concept.
- Do not pause for approval between routine implementation steps.
- Do not blindly force a genre template onto a custom project.
- Explain major architectural decisions in language the student can later explain to a judge.
- Use official Roblox documentation when an API, beta feature, limit, or deprecation may have changed.

## Core Quick Reference

### Service Hierarchy

- `ServerScriptService`: server-only logic, data ownership, anti-cheat.
- `ReplicatedStorage`: shared ModuleScripts, RemoteEvents, assets both client and server need.
- `StarterPlayerScripts`: client controllers for input, camera, and local UI behavior.
- `StarterGui`: ScreenGuis that clone into `PlayerGui`.
- `ServerStorage`: server-only assets and modules.
- `Workspace`: live 3D world; keep it organized and lean.

### Script Types

| Type | Runs On | Use For |
|---|---|---|
| `Script` | Server | Game logic, data, physics authority |
| `LocalScript` | Client | Input, camera, UI, local effects |
| `ModuleScript` | Either | Shared or service logic, config, utilities |

### RemoteEvent Basics

```luau
-- Server: listen
RemoteEvent.OnServerEvent:Connect(function(player, ...) end)
-- Client: fire
RemoteEvent:FireServer(...)
-- Server to client
RemoteEvent:FireClient(player, ...)
```

### Golden Rule

Never trust the client. Every RemoteEvent payload is attacker-controlled. Validate type, range, ownership, cooldown, and player state on the server for every request.

## Freshness Rule

When a request depends on current Roblox platform behavior, read `references/current-platform-lookup.md` and verify against official sources before writing implementation code.