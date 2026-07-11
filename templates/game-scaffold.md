# Universal Game Scaffold

Use this as the base for any Roblox game or app. It provides clean ownership boundaries without forcing persistence, monetization, or genre systems that the concept does not need.

## Scaffold Principles

- Server owns game state, validation, persistence, rewards, and anti-cheat.
- Client owns input, camera, local effects, and UI rendering.
- Shared modules contain constants, types, and pure helpers only.
- Persistence is optional. Add it only when the app needs progress after rejoin.
- ProfileStore is preferred for persistent player profiles.
- Raw DataStoreService is allowed for prototypes and non-profile data, but it is not session locked.

## Folder Structure

```text
game/
|-- ServerScriptService/
|   |-- Main.server.luau
|   `-- Services/
|       |-- GameService.luau
|       |-- RemoteService.luau
|       `-- PlayerDataService.luau       optional; only if persistence is needed
|-- ServerStorage/
|   |-- Modules/
|   `-- Assets/
|-- ReplicatedStorage/
|   |-- Shared/
|   |   |-- Constants.luau
|   |   `-- Types.luau
|   |-- Remotes/
|   `-- Assets/
|-- StarterGui/
|   `-- MainGui/
|-- StarterPlayer/
|   `-- StarterPlayerScripts/
|       `-- ClientController.client.luau
`-- Workspace/
    `-- Map/
```

## Placement Rules

| Content | Location |
|---|---|
| Server game logic, data, anti-cheat | `ServerScriptService/Services` |
| Server-only assets and private modules | `ServerStorage` |
| Shared constants, types, pure utilities | `ReplicatedStorage/Shared` |
| RemoteEvents and RemoteFunctions | `ReplicatedStorage/Remotes` |
| Client-visible assets | `ReplicatedStorage/Assets` |
| UI layouts | `StarterGui` |
| Client controllers | `StarterPlayerScripts` |
| Live world geometry | `Workspace/Map` |

## Constants Module

```luau
--!strict
local Constants = {
    REMOTE_WINDOW = 10,
    MAX_REMOTE_RATE = 30,
    INTERACTION_RANGE = 15,
    Remotes = {
        RequestAction = "RequestAction",
        UpdateUI = "UpdateUI",
    },
}

return Constants
```

## Remote Service Requirements

Every client-to-server remote must define:

- Expected argument types
- Player state checks
- Ownership or distance checks when relevant
- Cooldown or rate limit
- Server-side result calculation

Never trust client-submitted currency, damage, inventory, teleport, purchase, or completion values.

## Optional ProfileStore PlayerDataService

Only create this service when the concept needs saved progress. Use `references/datastore-persistence.md` for the full pattern.

Required ProfileStore lifecycle calls:

- `StartSessionAsync`
- `Cancel` when the player leaves during load
- `Reconcile`
- `OnSessionEnd`
- `EndSession`

Do not label a raw DataStore fallback as session locked. If ProfileStore is not installed and persistence is required, either add ProfileStore or clearly mark raw DataStore behavior as prototype-only.

## Build Order

1. Create folders and shared modules.
2. Create server services with no client trust.
3. Create remotes and validators.
4. Create client controller and UI shell.
5. Add Workspace objects needed for the vertical slice.
6. Add persistence only if required.
7. Playtest and read console output.

## Manual Setup Checklist

- Create the folder structure above.
- Add `Constants` and `Types` in `ReplicatedStorage/Shared`.
- Add `Main.server.luau` in `ServerScriptService`.
- Add services under `ServerScriptService/Services`.
- Add remotes under `ReplicatedStorage/Remotes`.
- Add `ClientController.client.luau` under `StarterPlayerScripts`.
- Add `MainGui` under `StarterGui` with `ResetOnSpawn = false` when the UI should persist.
- Press Play, verify the main action, and read Output for errors.

## Extension Points

| Extension Point | What To Add |
|---|---|
| `GameService` | Core loop, rules, round state, simulation |
| `RemoteService` | Remote definitions, validation, dispatch |
| `PlayerDataService` | Optional ProfileStore profile ownership |
| `Constants` | Shared tuning values and remote names |
| `Types` | Shared strict Luau types |
| `ClientController` | Input, local UI wiring, camera, local feedback |
| `MainGui` | HUD, menus, modals, notifications |
| `Workspace/Map` | Physical slice needed by gameplay |