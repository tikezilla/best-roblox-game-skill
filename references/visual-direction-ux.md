# Visual Direction and UX

Use this reference for professional Roblox UI, UX, animation, sound feedback, menus, HUDs, onboarding, and visual identity.

## Visual Direction Brief

Before major UI or scene polish, define:

1. Product promise: what the player should feel or understand in the first 10 seconds.
2. Audience: age, device, input style, and attention context.
3. Visual tone: 3-5 adjectives tied to the concept, not generic style words.
4. Motifs: repeated shapes, colors, materials, sounds, or motion patterns.
5. Screens: HUD, menu, modal, feedback, failure, success, and end state.
6. Constraints: mobile safe areas, readability, performance, and asset availability.

## Typography

Use one hierarchy, not random font choices:

| Role | Guidance |
|---|---|
| Title | Strongest weight, short labels, used sparingly |
| Section | Smaller than title, anchors panels or groups |
| Body | Readable, stable size, no tiny dense paragraphs on mobile |
| Numeric | Use consistent alignment for timers, scores, and counters |
| Label | Short, plain language; avoid decorative ambiguity |

Avoid scaling text purely by viewport width. Use explicit sizes, constraints, and layout rules that keep text readable across mobile and desktop.

## Spacing and Shape

- Pick one spacing scale, such as 4, 8, 12, 16, 24, 32.
- Use consistent corner rules: small controls, panels, chips, and cards should not all have unrelated radii.
- Use `UIPadding`, `UIListLayout`, `UIGridLayout`, `UISizeConstraint`, `UITextSizeConstraint`, and `UIAspectRatioConstraint` instead of manual guesswork.
- Use safe areas and avoid placing critical controls under mobile notches, system bars, or the Roblox top bar.

## Interaction States

Every interactive element needs visible behavior for:

- Default
- Hover or focus
- Press
- Disabled
- Loading
- Success
- Failure

Use `Activated` for buttons when possible because it works across mouse, touch, and gamepad. Use focus states and `GuiService` selection for gamepad navigation.

## Motion

Motion must communicate meaning:

- Opening a menu: establish hierarchy and origin.
- Success: confirm completion without blocking the player.
- Failure: identify the failed control and explain recovery.
- Loading: show that work is in progress.
- Navigation: preserve spatial orientation between screens.

Do not add random tweens to every element. Avoid infinite pulsing except for urgent, temporary states.

## Feedback

Every user action should produce immediate feedback through at least one channel:

- Visual: state change, highlight, progress, or notification.
- Sound: subtle confirmation, denial, success, or alert where appropriate.
- Haptic/input: if supported and appropriate.
- Text: short status copy when the result is not obvious.

Sound should support the interaction. Do not use loud or repetitive sound for routine UI updates.

## Responsive and Input Coverage

Plan for:

- Mobile touch
- Mouse and keyboard
- Gamepad
- Portrait and landscape layouts
- Small and large screens
- Safe areas and variable aspect ratios
- Reduced-motion preference when available

Verify UI at desktop and mobile dimensions. If screenshot capture exists, inspect the actual rendered UI before calling polish complete.

## Visual Identity Rules

- Do not use default gray Roblox interface unless the concept deliberately calls for it.
- Do not add decorative elements without a defined role.
- Repeat motifs and callbacks across screens so the experience feels coherent.
- Prefer a few strong visual decisions over many unrelated accents.
- In competition projects, make the purpose legible and memorable before adding spectacle.
