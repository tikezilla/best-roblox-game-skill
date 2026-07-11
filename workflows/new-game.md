# Workflow: New Game Creation

Use this workflow when the user wants to build a new Roblox game, app, prototype, simulator, school project, or custom experience.

## Default Behavior

Start with the concept, not the genre. Genre templates are tools, not identities. Use a simulator, tycoon, obby, RPG, horror, or battle-royale template only when it clearly matches part of the user's app.

For Congressional App Challenge or civic/school competition projects, route to `workflows/congressional-app-challenge.md` first.

## Step 1: Concept Brief

Define:

1. Purpose
2. Target user
3. Core action the player repeats
4. Five-minute playable experience
5. Visual tone
6. Data needs, if any
7. Three systems the user can explain

If the user already gave these details, summarize them and proceed. If a high-impact detail is missing, ask one concise question.

## Step 2: Project Shape

Inspect the current Studio project when MCP capabilities exist. If it is empty or the user wants a fresh build, use `templates/game-scaffold.md` as the base. If existing systems are present, switch to `workflows/existing-project-recovery.md` before building.

Use genre templates only for matching systems:

| Need | Optional Template |
|---|---|
| Click, collect, upgrade loop | `templates/genre-simulator.md` |
| Build base, income, expansion | `templates/genre-tycoon.md` |
| Stages, checkpoints, movement challenge | `templates/genre-obby.md` |
| Quests, stats, NPCs, progression | `templates/genre-rpg.md` |
| Atmosphere, monster, escape tension | `templates/genre-horror.md` |
| Rounds, eliminations, shrinking arena | `templates/genre-battle-royale.md` |

Avoid shops, currencies, retention systems, and monetization unless they support the product reason.

## Step 3: Architecture Approval

Before building, present one concise architecture for approval:

- Folder/Instance structure
- Script manifest with server/client/shared ownership
- RemoteEvents and RemoteFunctions
- Data schema or statement that persistence is not needed
- Playable vertical slice definition
- Test plan

Require one architecture approval. After approval, continue routine implementation without stopping for every small step.

## Step 4: Build Vertical Slice

Build the smallest playable experience first:

1. Shared constants/types
2. Server-owned gameplay state
3. Remote validation where client input is needed
4. Client input and UI feedback
5. Minimal Workspace or scene setup
6. Optional persistence only if required

Keep code explainable by the student or project owner.

## Step 5: Verify

For each completed slice:

1. Check immediate console output.
2. Start playtest.
3. Simulate or manually trigger the main user action.
4. Read runtime console output.
5. Capture screenshots for UI or visual work when possible.
6. Stop playtest.
7. Repair up to five cycles before reporting a blocker.

## Step 6: Expand

Only after the vertical slice works, add content, polish, and secondary systems. Keep each addition connected to the concept brief and test it independently.

## Summary Output

End with:

- What was built
- What was tested
- Any console errors or remaining risks
- What the user can try next in Studio
- What system the user should be able to explain