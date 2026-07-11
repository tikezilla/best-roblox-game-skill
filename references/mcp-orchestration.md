# MCP Studio Orchestration

Use this reference whenever Codex interacts with Roblox Studio through MCP, explores an existing place, changes scripts or instances, debug-tests behavior, or uses screenshots/input simulation for verification.

## Capability Detection

Inspect the tools exposed in the current session and map them by capability. Tool names below are examples, not required names.

| Capability | Possible tools |
|---|---|
| Studio sessions | `list_roblox_studios`, `set_active_studio`, `get_studio_state` |
| Search tree | `search_game_tree`, `get_file_tree`, `search_objects` |
| Inspect instance | `inspect_instance`, `get_instance_properties`, `get_instance_children` |
| Search source | `script_search`, `script_grep`, `grep_scripts` |
| Read scripts | `script_read`, `get_script_source` |
| Edit source | `multi_edit`, source-writing through Luau |
| Execute Luau | `execute_luau`, `run_code` |
| Asset work | `search_asset`, `insert_asset`, `generate_mesh`, `generate_material`, `generate_procedural_model`, `upload_image` |
| Test | `start_stop_play`, `start_playtest`, `stop_playtest` |
| Read errors | `get_console_output`, `get_playtest_output` |
| Simulate input | `character_navigation`, `user_mouse_input`, `user_keyboard_input` |
| Visual check | `screen_capture`, `store_image` |

Only use offline instructions when no Roblox Studio MCP capability is available.

## Multiple Studio Sessions

When session-management tools exist, list open Studio sessions before writing. Identify the intended writable session by place name, file path, active selection, user instruction, or visible project state. Set the intended session active, state it in the response, and treat every other Studio window as read-only reference material.

Never edit, insert, delete, playtest, or run destructive Luau in a reference session.

## Workflow

### ORIENT

1. Select the target Studio session.
2. Inspect the game tree.
3. Locate server, client, shared, UI, and Workspace entry points.
4. Read relevant scripts before proposing architecture or edits.
5. If tools cannot read a needed script directly, use Luau execution to print or locate it, then read console output.

### MAP

1. Map `require` dependencies and module ownership.
2. Map client-server communication through `RemoteEvent`, `RemoteFunction`, `BindableEvent`, and `BindableFunction`.
3. Identify physical Workspace references by name/path and any missing instances.
4. Identify persistent data ownership and save/load entry points.
5. Identify UI ownership: which LocalScripts own which ScreenGuis, HUDs, prompts, and notifications.

### PLAN

1. Define one coherent change or one playable vertical slice.
2. Record affected scripts and instances.
3. Create an undo waypoint when available before bulk or risky edits.
4. Prefer targeted edits over blind full-script replacement.
5. Ask before deleting substantial work or replacing architecture.

### BUILD

1. Use script-editing tools for script changes when available.
2. Use Luau execution for instance creation, inspection, small migrations, and verification helpers.
3. Batch related edits into a single coherent operation.
4. Keep secondary Studio sessions read-only.
5. Set `Parent` last when creating Instances in Luau.

### VERIFY

1. Check immediate console output after edits.
2. Start playtest when the change affects runtime behavior.
3. Simulate the relevant player action when input tools exist.
4. Read runtime console output.
5. Capture visual evidence for UI, 3D layout, camera, animation, or UX work when screenshot tools exist.
6. Stop playtest before applying persistent edit-mode fixes unless the tool explicitly supports the active mode.

### REPAIR

1. Read the failing source and exact console error.
2. Fix the root cause, not only the symptom.
3. Repeat verification.
4. Stop after five failed repair cycles and report the remaining failure, attempted fixes, and the next diagnostic step.

## Offline Mode

When no Studio tools are available, produce copy-paste-ready Luau with exact placement instructions.

```luau
-- SCRIPT: ExampleService
-- PLACE IN: ServerScriptService
-- TYPE: ModuleScript
-- PURPOSE: Owns server-side example behavior.
```

Include a setup checklist for required Instances, RemoteEvents, ModuleScripts, assets, and Studio settings.

## Safety Rules

- Read before write.
- Do not call `Destroy`, `ClearAllChildren`, or broad deletion loops on meaningful project content without explicit user approval.
- Do not modify the DataModel while playtest changes would be discarded, unless the target is intentionally runtime-only.
- Always check console output after changing scripts.
- Treat every RemoteEvent as hostile input until server validation proves otherwise.
- If documentation and an existing working project disagree, report the conflict before changing the project.