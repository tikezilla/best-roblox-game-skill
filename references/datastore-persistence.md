# Roblox Data Persistence Reference

Use this reference for DataStoreService, ProfileStore, leaderstats, player profile data, schema migration, and persistence decisions.

## Decision Rules

- Do not add persistence when the app does not need it.
- ProfileStore is preferred for persistent player profiles.
- Raw DataStoreService is allowed for small prototypes or non-profile data.
- Never describe raw GetAsync and UpdateAsync storage as session locked.
- Persistent data is server-owned. Clients may request actions, but only the server mutates saved state.
- Every DataStore call must be wrapped in `pcall` and designed for failure.
- Use official docs for current limits before writing hardcoded limit guidance.

## When To Skip Persistence

For a short prototype, civic simulator, classroom demo, or Congressional App Challenge vertical slice, ask whether progress must survive rejoin. If the answer is no, keep state in memory and focus on the playable experience, UX, and explainable systems.

## ProfileStore Player Profile Pattern

Use ProfileStore for production player profiles because it owns session lifecycle, reconciliation, auto-save, and server handoff behavior.

```luau
--!strict
-- ServerScriptService/PlayerDataService.server.luau
local Players = game:GetService("Players")
local ServerStorage = game:GetService("ServerStorage")

local ProfileStore = require(ServerStorage.ProfileStore)

local DATA_TEMPLATE = {
    coins = 0,
    level = 1,
    inventory = {},
    settings = {
        musicVolume = 0.5,
    },
    dataVersion = 1,
}

local playerStore = ProfileStore.New("PlayerData", DATA_TEMPLATE)
local profiles: { [Player]: any } = {}

local function onPlayerAdded(player: Player)
    local profile = playerStore:StartSessionAsync(`Player_{player.UserId}`, {
        Cancel = function()
            return player.Parent ~= Players
        end,
    })

    if profile == nil then
        player:Kick("Unable to load data. Please rejoin.")
        return
    end

    profile:AddUserId(player.UserId)
    profile:Reconcile()

    profile.OnSessionEnd:Connect(function()
        profiles[player] = nil
        player:Kick("Your data session ended. Please rejoin.")
    end)

    if player.Parent == Players then
        profiles[player] = profile
    else
        profile:EndSession()
    end
end

local function onPlayerRemoving(player: Player)
    local profile = profiles[player]
    if profile then
        profile:EndSession()
    end
end

Players.PlayerAdded:Connect(onPlayerAdded)
Players.PlayerRemoving:Connect(onPlayerRemoving)

for _, player in Players:GetPlayers() do
    task.spawn(onPlayerAdded, player)
end
```

Key lifecycle points:

- `StartSessionAsync` starts ownership for one player key.
- `Cancel` prevents loading data for a player who left during the async load.
- `Reconcile` fills missing template fields.
- `OnSessionEnd` reacts if the session is ended elsewhere.
- `EndSession` releases ownership on leave or abandoned load.

## Raw DataStore Pattern

Use raw DataStoreService only for small prototypes, global counters, non-profile data, or simple classroom demos where session ownership is not required.

```luau
--!strict
local DataStoreService = game:GetService("DataStoreService")
local store = DataStoreService:GetDataStore("PrototypeScores")

local function saveScore(userId: number, score: number): boolean
    local success, err = pcall(function()
        store:UpdateAsync(`Player_{userId}`, function(oldValue)
            oldValue = oldValue or 0
            return math.max(oldValue, score)
        end)
    end)

    if not success then
        warn("Score save failed:", err)
    end

    return success
end
```

`UpdateAsync` is atomic for a single key update, but that is not the same thing as a session lock. It does not prevent a second server from loading stale player profile data and later saving over newer in-memory state.

## Schema Guidance

- Store one profile table per player when possible.
- Include a `dataVersion` field.
- Store only serializable values: strings, numbers, booleans, tables, and buffers where supported.
- Never store Instances, Vector3, CFrame, Color3, or live Roblox objects directly.
- Reconcile defaults on load before gameplay systems read data.
- Keep migrations sequential and explainable.

## OrderedDataStore

Use OrderedDataStore for integer leaderboards only. It is not a profile store and should not own player inventory, settings, or progression.

## Studio Testing

- Enable Studio Access to API Services before testing DataStores in Studio.
- Use a development store name or prefix for test places.
- Test join, leave, rejoin, server shutdown, and failed load paths.
- Read console output for warnings after every persistence playtest.