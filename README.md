# roblox-game - Codex Roblox Skill

A Codex-first Roblox development skill for building, debugging, recovering, and polishing Roblox experiences with Luau and Roblox Studio MCP capabilities.

This hybrid keeps the broad structure of Brock Martin's original `roblox-game-skill` and selectively adapts stronger current guidance from MSayib's `roblox-dev-skill`. It is designed to be useful in Codex while staying compatible with generic Skills-style agents.

## What It Does

- Builds Roblox games and prototypes from concept briefs and playable vertical slices.
- Uses capability-based Roblox Studio MCP orchestration instead of fixed tool inventories.
- Recovers and maps inherited or broken projects before suggesting rewrites.
- Recovers inherited, decompiled, and partially rewritten projects by tracing behavior across scripts and runtime boundaries.
- Reconciles a rewritten server backend with legacy behavior without restoring damaged code wholesale.
- Guides ProfileStore-first persistence and clearly separates raw DataStore prototype use.
- Provides professional UI/UX, visual direction, asset format, migration, security, performance, and game design references.

## Structure

```text
roblox-game/
|-- SKILL.md
|-- metadata.json
|-- THIRD_PARTY_NOTICES.md
|-- agents/
|   `-- openai.yaml
|-- evals/
|   `-- evals.json
|-- references/
|   |-- mcp-orchestration.md
|   |-- datastore-persistence.md
|   |-- gui-systems.md
|   |-- visual-direction-ux.md
|   |-- current-platform-lookup.md
|   |-- legacy-migration.md
|   |-- file-formats-and-assets.md
|   `-- existing Brock references...
|-- workflows/
|   |-- new-game.md
|   |-- debug-loop.md
|   |-- existing-project-recovery.md
|   |-- reverse-engineered-recovery.md
|   `-- existing Brock workflows...
`-- templates/
    `-- existing Brock templates...
```

## Install For Codex

For a normal global Codex install, refresh with:

```powershell
npx skills add tikezilla/best-roblox-game-skill --agent codex --copy -y
```

Because this uses `--copy`, rerun the command after major repository updates.

For a project-only install, use the Codex skill installer with `--dest <project>/.agents/skills`, `--ref codex/bbai26-reconstruction`, and `--name roblox-game`. This keeps the tailored branch scoped to that project instead of changing the global skill set.

## Usage Examples

- "Inspect this old Roblox project and tell me what can be reused."
- "Infer the missing server behavior from these decompiled Roblox scripts."
- "Compare my rewritten backend with the old handlers and recover the missing contract."
- "Make this menu feel professionally designed and animated."
- "Fix this Studio project, but use the second open Studio window only as reference."
- "Add ProfileStore persistence for player progress."
- "Is this Roblox API still current'"

## Attribution

See `THIRD_PARTY_NOTICES.md` for source attribution and the MIT notice covering adapted MSayib material.
