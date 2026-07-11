# Roblox GUI/UI Systems Reference

Use this reference with `references/visual-direction-ux.md` for ScreenGui, HUDs, menus, in-world UI, responsive layout, interaction states, and current UI APIs.

## Core Containers

| Container | Use For |
|---|---|
| `ScreenGui` | 2D UI cloned from `StarterGui` into `PlayerGui` |
| `SurfaceGui` | UI rendered on a Part face |
| `BillboardGui` | Floating UI attached to a world object |
| `ViewportFrame` | 3D previews inside UI |
| `CanvasGroup` | Fading or transparency control for a UI subtree |

Set `ScreenGui.ResetOnSpawn = false` for persistent menus and HUD systems. Use `DisplayOrder` intentionally: HUD below notifications, notifications below modals.

## Layout Objects

Prefer layout-driven UI over hardcoded coordinates.

| Object | Purpose |
|---|---|
| `UIListLayout` | Vertical or horizontal lists |
| `UIGridLayout` | Inventories, shops, selectable tiles |
| `UIPageLayout` | Tutorial pages, tabbed flows, onboarding |
| `UIPadding` | Inner spacing |
| `UISizeConstraint` | Min/max pixel size |
| `UITextSizeConstraint` | Text size bounds |
| `UIAspectRatioConstraint` | Fixed visual ratios |
| `UIScale` | Uniform subtree scaling |
| `UIFlexItem` | Per-child grow/shrink with flexible layouts |

Use `UDim2.fromScale` for container placement and `UDim2.fromOffset` for icons, padding, strokes, and compact controls.

## Styling APIs

Use current styling features when they are available, with fallbacks for older Studio builds.

- `UICorner`: rounded corners; per-corner radii may require current UI capabilities.
- `UIStroke`: borders, outlines, selected/focus states.
- `UIGradient`: controlled gradients, not random decoration.
- `UIShadow`: native drop shadows under UI objects; prefer this over image shadow hacks when supported.
- `StyleSheet`, `StyleRule`, and `StyleQuery`: reusable styling, responsive conditions, and state rules.

Check `references/current-platform-lookup.md` before relying on a new or beta UI feature.

## Button Pattern

```luau
--!strict
local TweenService = game:GetService("TweenService")

local function wireButton(button: GuiButton, onActivated: () -> ())
    local baseSize = button.Size
    local hoverTween = TweenService:Create(button, TweenInfo.new(0.12), {
        BackgroundTransparency = 0.05,
    })
    local pressTween = TweenService:Create(button, TweenInfo.new(0.08), {
        Size = UDim2.new(baseSize.X.Scale, baseSize.X.Offset - 2, baseSize.Y.Scale, baseSize.Y.Offset - 2),
    })

    button.Activated:Connect(onActivated)
    button.MouseEnter:Connect(function()
        hoverTween:Play()
    end)
    button.MouseButton1Down:Connect(function()
        pressTween:Play()
    end)
    button.MouseButton1Up:Connect(function()
        button.Size = baseSize
    end)
end
```

Use `Activated` for the action so mouse, touch, and gamepad all work. Keep hover and mouse-only polish optional.

## HUD Pattern

- Keep HUD information compact and persistent.
- Anchor critical information away from safe-area conflicts.
- Use stable dimensions for counters so numbers do not shift the layout.
- Use color and iconography consistently for health, progress, warnings, and success.
- Do not hide gameplay under full-screen panels unless the player intentionally opened a modal.

## Menu Pattern

- Separate navigation, content, and action areas.
- Give every modal an obvious close/cancel path.
- Keep disabled states visibly different and non-clickable.
- For loading actions, disable repeat input and show progress or a spinner.
- For failure, keep the user on the same screen and show the recovery action.

## In-World UI

- Use `ProximityPrompt` for common world interactions unless custom presentation is required.
- Use `BillboardGui` for markers, names, health bars, and short labels.
- Use `SurfaceGui` for signs, terminals, and panels that are part of the world.
- Limit render distance and update frequency for many in-world elements.

## Responsive Verification

For every polished UI pass:

1. Inspect desktop landscape.
2. Inspect phone portrait or small landscape.
3. Verify mouse, touch, keyboard, and gamepad paths where relevant.
4. Check text wrapping, truncation, and minimum readable size.
5. Use screenshots when MCP supports capture.
6. Read console output after playtest for UI script errors.

## Performance and Cleanup

- Parent UI trees last when creating many objects in code.
- Reuse item cards in large scrolling lists instead of destroying and recreating every frame.
- Disconnect event connections when a UI is destroyed.
- Prefer `Visible` or `ScreenGui.Enabled` toggles over rebuilding unchanged UI.
- Avoid expensive per-frame text/layout updates.